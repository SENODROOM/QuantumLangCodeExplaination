# TypeChecker.h — Static Type Analysis

The type checker is an optional pass between parsing and interpretation. It walks the AST and infers or validates types for declarations and expressions, reporting errors before the program ever runs. It is intentionally shallow: the goal is to catch the most common mistakes (wrong type in a typed declaration, use of an unknown variable) without requiring full type annotations on every expression.

---

## `StaticTypeError`

```cpp
class StaticTypeError : public std::runtime_error {
public:
    int line;
    StaticTypeError(const std::string& msg, int l)
        : std::runtime_error(msg), line(l) {}
};
```

`StaticTypeError` is kept separate from the runtime `QuantumError` hierarchy because it is raised in a different phase and caught by different code. The main entry point catches `StaticTypeError` and reports it before launching the interpreter — a static type error should never reach the interpreter at all.

The class is minimal: a message string (inherited from `std::runtime_error`) and a source line number. There is no `kind` field unlike `QuantumError`, because there is only one category of static error. If the type checker is extended to produce warnings as well as errors, a `severity` field would be the natural addition.

---

## `TypeEnv` — the type environment

```cpp
struct TypeEnv {
    std::map<std::string, std::string> vars;
    std::shared_ptr<TypeEnv> parent;

    TypeEnv(std::shared_ptr<TypeEnv> p = nullptr) : parent(p) {}

    void define(const std::string& name, const std::string& type) {
        vars[name] = type;
    }

    std::string resolve(const std::string& name) {
        if (vars.count(name)) return vars[name];
        if (parent) return parent->resolve(name);
        return "any";
    }
};
```

`TypeEnv` mirrors the runtime `Environment` class but stores type strings instead of `QuantumValue` objects. The same parent-chain scoping model is used: `resolve()` walks up the chain until it finds the variable or reaches the root. If a variable is not found anywhere in the chain, it returns `"any"` rather than throwing an error — this is the type checker's graceful-degradation policy for unrecognized names.

**Why `std::map` rather than `std::unordered_map`?** The runtime `Environment` uses `unordered_map` for O(1) average-case lookup. The type checker uses `std::map` (sorted, O(log n)). Since type checking runs once at startup rather than on every statement execution, the performance difference is negligible, and `std::map` has deterministic iteration order which helps when debugging type environment state.

**Types are strings.** The type system represents types as plain strings: `"float"`, `"string"`, `"bool"`, `"fn"`, `"void"`, `"any"`. This is the simplest possible representation and is appropriate for a shallow type checker. A richer type system (generics, union types, function signatures) would require a dedicated `Type` algebraic data type, but that is a significant complexity increase for uncertain benefit in a dynamically-typed language.

The special type `"any"` acts as the type checker's escape hatch. Any operation involving an `"any"`-typed value is permitted without error. This means unannotated variables, variables from unresolved scopes, and return values of most native functions all have type `"any"`, making the type checker opt-in rather than all-or-nothing.

---

## `TypeChecker` — the main class

```cpp
class TypeChecker {
public:
    TypeChecker();
    void check(const std::vector<ASTNodePtr>& nodes);
    void check(const ASTNodePtr& node);
    std::string checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env);

private:
    std::shared_ptr<TypeEnv> globalEnv;
};
```

### Constructor

`TypeChecker()` initializes `globalEnv` and pre-registers the types of built-in functions. For example, `len` is registered as a function that returns `"float"` (since all numbers are `double`), `typeof` returns `"string"`, and `print` returns `"void"`. This ensures that calls to built-ins don't produce spurious "unknown function" warnings.

### `check()` — entry points

Two overloads:
- `check(const vector<ASTNodePtr>& nodes)` — the top-level entry point, iterates over the program's statement list and calls `check(node)` for each.
- `check(const ASTNodePtr& node)` — calls `checkNode(node, globalEnv)`.

The split exists so that `check(nodes)` can be called from `main.cpp` with the full program, while individual nodes can be checked in isolation for testing.

### `checkNode()` — the core

```cpp
std::string checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env);
```

`checkNode` is the type checker's equivalent of the interpreter's `evaluate()`. It takes a node and the current type environment, and returns the inferred type of the node as a string. For statement nodes it returns `"void"`. For expression nodes it returns the inferred type of the expression.

Internally, `checkNode` uses `std::visit` on the node's variant, dispatching to per-node-type logic. The key cases:

**`VarDecl`** — if the declaration has a type hint (`int x = expr`), `checkNode` infers the type of `expr` and verifies it matches the hint. A mismatch throws `StaticTypeError`. If there is no hint, the inferred type of `expr` is stored as the variable's type in `env`. If there is no initializer either, the variable gets type `"any"`.

**`FunctionDecl`** — the function is registered in `env` with type `"fn"`. The body is checked in a new child `TypeEnv` with the parameters defined. Parameter types come from type annotations if present, otherwise `"any"`.

**`BinaryExpr`** — both operands are checked recursively. If both have the same concrete type (e.g. both `"float"`), the result type is that type. If either is `"any"`, the result is `"any"`. A small number of obviously invalid combinations (e.g. `"bool" + "string"`) may raise a `StaticTypeError`, but most combinations pass as `"any"` to avoid false positives.

**`Identifier`** — looks up the name in `env` via `resolve()`. If `resolve()` returns `"any"` (name not found), no error is raised — the type checker defers to the runtime for unknown names.

---

## How it integrates into the pipeline

```
Parser → vector<ASTNodePtr> → TypeChecker::check() → Interpreter::execute()
```

The type checker does not modify the AST. It reads the tree, building up a `TypeEnv` in parallel with the AST's structure, and either completes silently (no errors) or throws `StaticTypeError` at the first violation. The AST passed to the interpreter is the same object that was passed to the type checker.

Type checking is currently opt-in: `main.cpp` enables it based on a command-line flag or a pragma in the source file. If the type checker is not run, the program executes with full dynamic typing. This means a program can be developed without annotations and then progressively annotated over time.

---

## Current limitations and honest assessment

The type system is deliberately shallow. Things it catches:

- `int x = "hello"` — assigning a string to an explicitly int-typed variable
- `const x = 5; x = 6` — mutation of a const variable (caught by the interpreter, not the type checker, via `constants` in `Environment`)
- Calls to undefined functions (if the function name is not in any reachable `TypeEnv`)

Things it does not catch:

- Type errors in function return values (return type checking is not implemented)
- Type errors through containers (`arr.push("hello")` on an int array)
- Type errors through indirect references (`*ptr = "wrong"`)
- Any error involving `"any"`-typed intermediates, which is most of them in unannotated code

A full type checker for this language would need to track function return types, container element types, and pointer target types — a significantly larger undertaking than the current pass.