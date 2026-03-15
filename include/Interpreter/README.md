# Interpreter.h — The Interpreter Interface

The `Interpreter` class is the execution engine of the Quantum Language runtime. It takes a tree of `ASTNode` objects produced by the parser and executes them by walking the tree recursively. This header defines the public surface of the interpreter and the private method layout that mirrors the structure of the AST.

---

## Public interface

```cpp
class Interpreter {
public:
    Interpreter();
    void execute(ASTNode& node);
    QuantumValue evaluate(ASTNode& node);
    void execBlock(BlockStmt& s, std::shared_ptr<Environment> scope = nullptr);

    std::shared_ptr<Environment> globals;
    ...
};
```

Only three methods and one field are public:

- **`execute(node)`** — runs a statement node. Does not return a value. Internally dispatches to the appropriate `exec*` method based on the node's type.
- **`evaluate(node)`** — evaluates an expression node and returns a `QuantumValue`. Internally dispatches to the appropriate `eval*` method.
- **`execBlock(s, scope)`** — executes a sequence of statements. The optional `scope` parameter allows the caller to inject a pre-built environment (used when entering function bodies, where the function's argument bindings are already set up before execution begins).
- **`globals`** — the top-level environment. Exposed publicly so the test runner and REPL can inspect or pre-populate global variables without going through the interpreter's execution path.

`execute` and `evaluate` are separated rather than unified into a single `eval` method because statements and expressions have a fundamentally different contract: expressions always produce a value, statements never do. Keeping the types distinct at the C++ level prevents the interpreter from accidentally using a statement result as a value.

---

## Infinite-loop guard

```cpp
private:
    long long stepCount_ = 0;
    static constexpr long long MAX_STEPS = 2'000'000;
```

The interpreter counts execution steps and throws a `RuntimeError` if `MAX_STEPS` is exceeded. This is essential for the test runner, which runs programs without interactive stdin — a program that loops waiting for input would otherwise hang the test process indefinitely. Two million steps is generous enough for real programs but tight enough to catch runaway loops quickly.

---

## Statement executors (private)

Each `exec*` method handles exactly one statement node type:

| Method | Handles |
|--------|---------|
| `execVarDecl` | `let`/`const` declarations; type-checks against hint if present |
| `execFunctionDecl` | Wraps params + body in a `QuantumFunction` and binds it in the current scope |
| `execClassDecl` | Builds a `QuantumClass` object from method/field lists and stores it as a value |
| `execIf` | Evaluates the condition and branches; recursively handles elif/else |
| `execWhile` | Loops until condition is false or a `BreakSignal` is thrown |
| `execFor` | Iterates over arrays, strings, dicts, and tuple-unpacks where `var2` is set |
| `execReturn` | Throws a `ReturnSignal` carrying the return value up the call stack |
| `execPrint` | Evaluates all args, joins with `sep`, appends `end` |
| `execInput` | Reads a line from stdin; assigns to either a simple name or a complex lvalue |
| `execImport` | Loads a `.sa` module file, runs it in an isolated interpreter, and copies its globals |
| `execExprStmt` | Calls `evaluate()` and discards the result |

**Control flow via exceptions.** `return`, `break`, and `continue` are implemented as C++ exceptions (`ReturnSignal`, `BreakSignal`, `ContinueSignal`). This is the standard approach for tree-walking interpreters: rather than threading a "did-we-return?" flag through every method's return value, a signal exception unwinds the call stack directly to the loop or function boundary that catches it. It is slower than flag-threading in pathological cases but produces much cleaner code.

---

## Expression evaluators (private)

| Method | Handles |
|--------|---------|
| `evalBinary` | Arithmetic, comparison, logical, and bitwise operators; also string concatenation |
| `evalUnary` | `-`, `!`, `~` |
| `evalAssign` | `=` and compound assignment (`+=`, `-=`, etc.); delegates target-setting to `setLValue` |
| `evalCall` | Resolves the callee to a function/native/class and dispatches to the right call handler |
| `evalIndex` | Array/string/dict indexing; also handles `SliceExpr` |
| `evalMember` | Dot access on instances, dicts, and built-in types |
| `evalArray` | Evaluates each element and constructs an `Array` |
| `evalDict` | Evaluates each key-value pair and constructs a `Dict` |
| `evalLambda` | Captures the current environment and wraps it in a `QuantumFunction` (closure) |
| `evalListComp` | Iterates the iterable, applies the optional filter, collects the result expression |
| `evalIdentifier` | Walks the environment chain to find a variable's value |

### Pointer evaluators

```cpp
QuantumValue evalAddressOf(AddressOfExpr& e); // &var
QuantumValue evalDeref(DerefExpr& e);         // *ptr
QuantumValue evalArrow(ArrowExpr& e);         // ptr->member
QuantumValue evalNewExpr(NewExpr& e);         // new T(args) / new T[n]
```

These implement C++-style pointer semantics within the interpreter. `evalAddressOf` returns a `QuantumPointer` that holds a reference to the variable's storage cell in the environment, rather than a copy of its value. `evalDeref` reads through that pointer. This is how reference parameters (`int& r`) work: the caller passes the address of the variable, and the function body dereferences it implicitly on each access.

---

## Function call dispatch

Three private methods handle different kinds of callables:

```cpp
QuantumValue callFunction(shared_ptr<QuantumFunction>, vector<QuantumValue> args);
QuantumValue callNative(shared_ptr<QuantumNative>, vector<QuantumValue> args);
QuantumValue callInstanceMethod(shared_ptr<QuantumInstance>, shared_ptr<QuantumFunction>, vector<QuantumValue> args);
```

`evalCall` resolves the callee's type and dispatches to the appropriate one. `callFunction` handles user-defined functions: it creates a new environment scoped to the function, binds arguments (checking `paramIsRef` to decide whether to bind by value or by reference), and calls `execBlock`. `callNative` simply invokes the underlying C++ lambda. `callInstanceMethod` is like `callFunction` but additionally binds `this` in the function's scope.

---

## Built-in method dispatch

```cpp
QuantumValue callMethod(QuantumValue& obj, const string& method, vector<QuantumValue> args);
QuantumValue callArrayMethod(shared_ptr<Array>, const string& method, ...);
QuantumValue callStringMethod(const string&, const string& method, ...);
QuantumValue callDictMethod(shared_ptr<Dict>, const string& method, ...);
```

`evalMember` detects when the result of a dot-access is being called and routes to `callMethod`, which then branches on the object's type. This is how `arr.push(x)`, `str.split(",")`, and `dict.keys()` are implemented — as hardcoded cases in `callArrayMethod`, `callStringMethod`, and `callDictMethod` respectively. Adding a new built-in method requires only adding a case in the appropriate dispatch method.

---

## `setLValue` — assignment targets

```cpp
void setLValue(ASTNode& target, QuantumValue val, const string& op);
```

Assignment in Quantum Language can target more than just a variable name: `arr[i] = x`, `obj.field = x`, and `*ptr = x` are all valid. `setLValue` handles all of these by inspecting the target node's type and performing the appropriate write operation — environment update for identifiers, array mutation for index expressions, and pointer dereference-and-assign for deref expressions. The `op` parameter carries the operator (`=`, `+=`, etc.) so compound assignment is resolved here rather than requiring the caller to pre-compute the new value.