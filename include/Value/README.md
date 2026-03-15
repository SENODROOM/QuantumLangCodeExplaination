# Value.h — The Runtime Value System

This header is the foundation of the interpreter. It defines every type a Quantum Language value can have at runtime, how values are stored and passed around, how the environment manages variable bindings, how classes and instances are represented, and how control flow signals work. Everything the interpreter touches at runtime is defined here.

---

## `QuantumValue` — the universal value type

```cpp
struct QuantumValue {
    using Data = std::variant<
        QuantumNil,
        bool,
        double,
        std::string,
        std::shared_ptr<Array>,
        std::shared_ptr<Dict>,
        std::shared_ptr<QuantumFunction>,
        std::shared_ptr<QuantumNative>,
        std::shared_ptr<QuantumInstance>,
        std::shared_ptr<QuantumClass>,
        std::shared_ptr<QuantumPointer>>;

    Data data;
    ...
};
```

Every value in the runtime — a number, a string, a function, an object instance — is a `QuantumValue`. The active type is tracked by `std::variant`, which stores exactly one of the listed types at a time and remembers which one.

### Why variant, not inheritance?

An alternative design would be a base class `Value` with derived classes `NumberValue`, `StringValue`, etc., and virtual dispatch. The variant approach has two advantages here:

1. **No extra heap allocation.** With inheritance, every value must be heap-allocated as its derived type and accessed through a base pointer. With variant, small values like `bool` and `double` are stored directly inside `QuantumValue` — no allocation, no pointer indirection.
2. **Exhaustive checking.** `std::visit` on a variant forces every possible type to be handled. Adding a new type to `Data` without updating every `std::visit` call site is a compile error.

The tradeoff is that `QuantumValue` is as large as its largest member. With `std::string` in the variant (typically 24–32 bytes on 64-bit systems), every `QuantumValue` is at least 32 bytes. Values that don't need that space (booleans, nil) still occupy the same memory.

### Value types in detail

**`QuantumNil`** — an empty struct representing `nil`. The default constructor `QuantumValue()` produces nil. `isNil()` checks for it.

**`bool`** — stored directly, not boxed. The `explicit QuantumValue(bool b)` constructor prevents accidental construction from integers (without `explicit`, `QuantumValue(0)` would be ambiguous between `bool` and `double`).

**`double`** — all numeric values, both integer and floating-point, are stored as `double`. This unifies the number type, simplifying arithmetic operations (no `int`/`float` dispatch). The cost is precision loss for integers larger than 2^53 (~9 quadrillion), which is outside the range of typical cybersecurity script values.

**`std::string`** — strings are stored by value inside the variant. Assigning one `QuantumValue` string to another copies the string. This is value semantics, matching Python's immutable string behavior: modifying one variable's string value never affects another variable that previously held the same string.

**`shared_ptr<Array>` and `shared_ptr<Dict>`** — arrays and dictionaries use reference semantics. `Array` is `vector<QuantumValue>`; `Dict` is `unordered_map<string, QuantumValue>`. When a `QuantumValue` holding an array is assigned to another variable, both variables share the same underlying `vector`. Mutations through either variable are visible to the other, matching Python's mutable container semantics.

**`shared_ptr<QuantumFunction>`** — user-defined functions. Passed by shared pointer so closures work correctly: a lambda that captures a function and stores it can safely outlive the scope where the function was defined.

**`shared_ptr<QuantumNative>`** — C++ functions registered in `registerNatives()`. The `fn` field is a `std::function<QuantumValue(vector<QuantumValue>)>`, which can hold any callable including lambdas. `isFunction()` returns true for both `QuantumFunction` and `QuantumNative`, so call sites don't need to distinguish at the check level.

**`shared_ptr<QuantumInstance>` and `shared_ptr<QuantumClass>`** — class definitions and object instances. Classes are values that can be passed around and stored in variables; calling a class value creates an instance.

**`shared_ptr<QuantumPointer>`** — a pointer/reference to a variable's storage cell, described below.

### Constructors

Every type has a corresponding `explicit` constructor. `explicit` prevents implicit conversions — without it, a call like `callFunction(fn, {true})` could accidentally construct a `QuantumValue` from a `shared_ptr<QuantumFunction>` where a `bool` was intended. The move constructors for `string` and all `shared_ptr` types avoid redundant reference count increments.

### Type checks and accessors

Each type has a paired `is*()` and `as*()` method:

```cpp
bool isNumber() const { return std::holds_alternative<double>(data); }
double asNumber() const { return std::get<double>(data); }
```

`as*()` uses `std::get`, which throws `std::bad_variant_access` if the type is wrong. The interpreter always calls `is*()` before `as*()` in cases where the type is uncertain. In cases where the type is known from context (e.g. inside `evalBinary` after already checking `isNumber()`), calling `as*()` directly is safe and avoids a redundant check.

`isTruthy()`, `toString()`, and `typeName()` are declared in the header but defined in `Value.cpp`. `isTruthy()` determines the boolean interpretation of any value (nil → false, 0.0 → false, "" → false, empty array → true, everything else → true). `typeName()` returns a string like `"int"`, `"float"`, `"string"`, `"array"` used in error messages.

---

## `QuantumFunction` — user-defined functions and closures

```cpp
struct QuantumFunction {
    std::string name;
    std::vector<std::string> params;
    std::vector<bool> paramIsRef;
    std::vector<ASTNode*> defaultArgs;
    ASTNode* body;                   // non-owning ptr
    std::shared_ptr<Environment> closure;
};
```

When a function is declared, the interpreter creates a `QuantumFunction` and stores it as a `QuantumValue` in the current scope. The crucial field is `closure`: it holds a `shared_ptr` to the environment that was active at the time of declaration. When the function is later called, a new scope is created with `closure` as its parent — this is what makes closures work.

`body` is a raw (non-owning) pointer to the `ASTNode` that was parsed for the function body. The AST owns the node through `unique_ptr` chains; the `QuantumFunction` just borrows a view of it. This is safe as long as the AST outlives all function values derived from it, which is guaranteed because the AST lives in `main.cpp`'s stack frame for the duration of execution.

`defaultArgs` is a parallel vector of raw `ASTNode*` pointers (or `nullptr` for parameters without defaults). Default expressions are re-evaluated on each call, matching Python's semantics for call-time default evaluation. (Python actually evaluates defaults at definition time for the common `def f(x=[]):` footgun; Quantum avoids this by re-evaluating each time.)

---

## `QuantumPointer` — references and pointer arithmetic

```cpp
struct QuantumPointer {
    std::shared_ptr<QuantumValue> cell;  // live reference to variable storage
    std::string varName;                 // for display/debug
    int offset = 0;                      // for pointer arithmetic

    bool isNull() const { return cell == nullptr; }

    QuantumValue& deref() const {
        if (!cell) throw std::runtime_error("Null pointer dereference");
        return *cell;
    }
};
```

`QuantumPointer` provides the mechanism behind the `&var` address-of operator. When code writes `&x`, the interpreter calls `Environment::getCell("x")`, which promotes the variable's storage to a `shared_ptr<QuantumValue>` and returns it. The `QuantumPointer` holds that `shared_ptr`. Dereferencing the pointer (`*ptr`) returns a reference to the live storage, so writes through the pointer affect the original variable.

The `offset` field supports basic pointer arithmetic (`ptr + 1`). This is currently limited — it doesn't yet implement C-style array-pointer equivalence — but the field is present for future use.

`varName` is stored purely for error messages and debug output: `Null pointer dereference: 'x'` is more useful than `Null pointer dereference`.

---

## `Environment` — variable scoping

```cpp
class Environment : public std::enable_shared_from_this<Environment> {
public:
    explicit Environment(std::shared_ptr<Environment> parent = nullptr);

    void define(const std::string& name, QuantumValue val, bool isConst = false);
    void defineRef(const std::string& name, std::shared_ptr<QuantumValue> cell);
    QuantumValue get(const std::string& name) const;
    void set(const std::string& name, QuantumValue val);
    bool has(const std::string& name) const;
    const std::unordered_map<std::string, QuantumValue>& getVars() const;

    std::shared_ptr<QuantumValue> getCell(const std::string& name);

    std::shared_ptr<Environment> parent;

private:
    std::unordered_map<std::string, QuantumValue> vars;
    std::unordered_map<std::string, bool> constants;
    std::unordered_map<std::string, std::shared_ptr<QuantumValue>> cells;
};
```

`Environment` inherits `enable_shared_from_this` so it can produce a `shared_ptr<Environment>` to itself (`shared_from_this()`) when needed — this is used when capturing the current scope as a closure.

### Three storage maps

The environment has three parallel maps keyed by variable name:

- **`vars`** — the primary value store. `get()` reads from here (and parent chain). `set()` writes to the innermost scope that already defines the name.
- **`constants`** — a flag per variable name. Set to `true` when `define()` is called with `isConst = true`. `set()` checks this map and throws a `RuntimeError` if a write to a const is attempted.
- **`cells`** — created lazily by `getCell()`. When code first takes the address of a variable (`&x`), `getCell("x")` promotes the variable's entry from `vars` into a `shared_ptr<QuantumValue>` stored in `cells`. From that point forward, reads and writes to `x` go through the cell. This ensures that any pointer to `x` sees the same storage as a direct access to `x`.

### `defineRef()` — reference parameters

```cpp
void defineRef(const std::string& name, std::shared_ptr<QuantumValue> cell);
```

When a reference parameter (`int& r`) is bound at a call site, the interpreter obtains the caller's cell for the argument variable and calls `defineRef("r", cell)` in the function's scope. This inserts the cell directly into `cells`, bypassing `vars`. Subsequent reads and writes to `r` in the function body go through the shared cell and directly affect the caller's variable.

---

## `QuantumClass` and `QuantumInstance`

```cpp
struct QuantumClass {
    std::string name;
    std::shared_ptr<QuantumClass> base;
    std::unordered_map<std::string, std::shared_ptr<QuantumFunction>> methods;
    std::unordered_map<std::string, std::shared_ptr<QuantumFunction>> staticMethods;
    std::unordered_map<std::string, QuantumValue> staticFields;
};

struct QuantumInstance {
    std::shared_ptr<QuantumClass> klass;
    std::unordered_map<std::string, QuantumValue> fields;
    std::shared_ptr<Environment> env;

    QuantumValue getField(const std::string& name) const;
    void setField(const std::string& name, QuantumValue val);
};
```

`QuantumClass` stores the class's name, a pointer to its base class (for single inheritance), its instance methods, its static methods, and its static fields. Instance methods are stored as `QuantumFunction` objects rather than raw `ASTNode*` chains, so a class value is fully self-contained — it doesn't need the AST to dispatch method calls.

`QuantumInstance` holds a back-pointer to its class (used to look up methods), a per-instance field map, and an `env` — the scope the instance was created in, used when bound methods need to capture the instance's context.

Method resolution follows the inheritance chain: `callInstanceMethod` first checks `klass->methods` for the method name; if not found, it walks `klass->base` upward until found or until the base chain is exhausted.

---

## Control flow signals

```cpp
struct ReturnSignal { QuantumValue value; explicit ReturnSignal(QuantumValue v) : value(std::move(v)) {} };
struct BreakSignal {};
struct ContinueSignal {};
```

These three structs are thrown as C++ exceptions to implement non-local control flow. They are not `std::exception` subclasses — they are not errors. They are purely a mechanism for unwinding the call stack to a specific catch point:

- `ReturnSignal` is caught by `callFunction()`, which extracts the return value.
- `BreakSignal` is caught by `execWhile()` and `execFor()`, which exit their loop.
- `ContinueSignal` is caught by `execWhile()` and `execFor()`, which skip to the next iteration.

Using exceptions for this is standard practice for tree-walking interpreters. The alternative — threading a `ControlFlow` enum through every `execute()` return value — would require every statement executor to propagate the flag upward, adding conditionals to every level of the call stack. The exception approach is cleaner and makes the code at each level independent of whether it's inside a loop or function.