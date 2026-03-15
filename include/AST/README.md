# AST.h — Abstract Syntax Tree

The AST is the central data structure of the Quantum Language compiler. After the lexer tokenizes source text and the parser validates its grammar, the result is a tree of `ASTNode` objects — one node per syntactic construct. The interpreter then walks this tree to execute the program. Nothing in the runtime ever touches raw source text; it works entirely through the AST.

---

## Memory model: why `unique_ptr`

Every child node is stored as `ASTNodePtr`, which is `std::unique_ptr<ASTNode>`. Unique ownership means that when a parent node is destroyed, all its children are automatically freed — no manual `delete`, no garbage collector. This is RAII applied to the entire tree: constructing a node is allocation, and destroying it is deallocation.

```cpp
using ASTNodePtr = std::unique_ptr<ASTNode>;
```

The one exception is `TryStmt` and `ExceptClause`, which use `shared_ptr<ASTNode>` for their bodies. This is because exception handler bodies may need to be referenced from multiple places during error recovery.

---

## The variant design: one type, every node

Instead of a traditional class hierarchy with a virtual `eval()` method on every node, this AST uses `std::variant`:

```cpp
using NodeVariant = std::variant<
    NumberLiteral, StringLiteral, BoolLiteral, NilLiteral,
    Identifier,
    BinaryExpr, UnaryExpr, AssignExpr,
    CallExpr, IndexExpr, SliceExpr, MemberExpr,
    ArrayLiteral, DictLiteral, LambdaExpr, ListComp, TupleLiteral,
    VarDecl, FunctionDecl, ReturnStmt,
    IfStmt, WhileStmt, ForStmt,
    BlockStmt, ExprStmt,
    PrintStmt, InputStmt,
    BreakStmt, ContinueStmt, SuperExpr,
    RaiseStmt, TryStmt,
    ImportStmt, ClassDecl,
    TernaryExpr,
    AddressOfExpr, DerefExpr, ArrowExpr,
    NewExpr>;

struct ASTNode {
    NodeVariant node;
    int line = 0;
    ...
};
```

**Why variant over virtual dispatch?**

- **No heap allocation per node type.** A virtual class hierarchy requires every node to be heap-allocated as its derived type. With variant, the node data is stored inline inside `ASTNode` — the size of the variant is just the size of the largest member.
- **Exhaustive dispatch.** When the interpreter calls `std::visit` on a `NodeVariant`, the compiler enforces that every node type is handled. Adding a new AST node and forgetting to handle it in the interpreter is a compile-time error, not a silent runtime bug.
- **Better cache locality.** Nodes are stored contiguously in their parent's `vector<ASTNodePtr>` pointer array rather than scattered across the heap via vtable pointers.

The tradeoff is that adding a new node type requires recompiling every `std::visit` call site. For a compiler where the node set is stable, this is the right trade.

---

## Expression nodes

| Node | Represents | Key fields |
|------|-----------|------------|
| `NumberLiteral` | A numeric constant | `double value` |
| `StringLiteral` | A quoted string | `string value` |
| `BoolLiteral` | `true` / `false` | `bool value` |
| `NilLiteral` | `nil` / null | *(empty)* |
| `Identifier` | A variable reference | `string name` |
| `BinaryExpr` | `a + b`, `a == b`, etc. | `string op`, `left`, `right` |
| `UnaryExpr` | `-a`, `!a` | `string op`, `operand` |
| `AssignExpr` | `x = 5`, `x += 1` | `string op`, `target`, `value` |
| `CallExpr` | `f(a, b)` | `callee`, `vector<> args` |
| `IndexExpr` | `arr[i]` | `object`, `index` |
| `SliceExpr` | `arr[1:5:2]` | `object`, `start`, `stop`, `step` (any may be null) |
| `MemberExpr` | `obj.field` | `object`, `string member` |
| `ArrayLiteral` | `[1, 2, 3]` | `vector<> elements` |
| `DictLiteral` | `{"k": "v"}` | `vector<pair<>> pairs` |
| `LambdaExpr` | `fn(x) => x*2` | `params`, `defaultArgs`, `body` |
| `ListComp` | `[x*2 for x in arr]` | `expr`, `vars`, `iterable`, `condition` |
| `TupleLiteral` | `(a, b)` | `vector<> elements` |
| `TernaryExpr` | `a if cond else b` | `condition`, `thenExpr`, `elseExpr` |
| `SuperExpr` | `super()` or `super.method()` | `string method` (empty = constructor) |

### Pointer/C++-style expression nodes

These bring low-level pointer semantics into the language:

| Node | Operator | Purpose |
|------|---------|---------|
| `AddressOfExpr` | `&var` | Capture a reference to a variable's storage location |
| `DerefExpr` | `*ptr` | Read through a pointer/reference |
| `ArrowExpr` | `ptr->member` | Member access through a pointer |
| `NewExpr` | `new T(args)` or `new T[n]` | Heap allocation; `isArray` distinguishes the two forms |

---

## Statement nodes

| Node | Represents |
|------|-----------|
| `VarDecl` | `let`/`const` declaration, with optional type hint (`int x`) and pointer flag (`int* p`) |
| `FunctionDecl` | Function with params, per-param ref flags (`paramIsRef`), defaults, and a `BlockStmt` body |
| `ClassDecl` | Class with optional base, methods, static methods, and field declarations |
| `IfStmt` | Condition + then branch + optional else (elif chains are nested as else-if) |
| `WhileStmt` | Condition + body |
| `ForStmt` | Python-style `for var in iterable` with optional `var2` for tuple unpacking (`for k, v in ...`) |
| `BlockStmt` | A sequence of statements (function bodies, loop bodies, etc.) |
| `ExprStmt` | An expression used as a statement (e.g., a bare function call) |
| `PrintStmt` | `print(...)` with configurable `sep` and `end`, matching Python's signature |
| `InputStmt` | `input(prompt)` assigned to either a simple variable or a complex lvalue like `arr[i]` |
| `ReturnStmt` | `return` with optional value |
| `BreakStmt` / `ContinueStmt` | Loop control (empty structs — their presence in the tree is the information) |
| `RaiseStmt` | `raise expr` — throws a runtime exception |
| `TryStmt` | `try` body + list of `ExceptClause` handlers + optional `finally` body |
| `ImportStmt` | `from module import name as alias` — `module` is empty for bare `import` |

### Why `FunctionDecl` carries `paramIsRef`

Pass-by-reference parameters (`int& r`) are a syntactic distinction that must survive from the parser all the way to the interpreter's function-call logic. Rather than encoding this in a wrapper type or a separate AST node, the parallel `vector<bool> paramIsRef` flag array lets the interpreter check `paramIsRef[i]` when binding each argument, which keeps the dispatch path simple.

---

## The `ASTNode` wrapper

```cpp
struct ASTNode {
    NodeVariant node;
    int line = 0;

    template <typename T>
    ASTNode(T&& n, int ln = 0) : node(std::forward<T>(n)), line(ln) {}

    template <typename T>
    T& as() { return std::get<T>(node); }

    template <typename T>
    bool is() const { return std::holds_alternative<T>(node); }
};
```

Every node in the tree carries its source line number. This is used by the error system to produce messages like `TypeError at line 42` — the interpreter doesn't need a separate source-map structure because the line is embedded in the node itself.

The `as<T>()` accessor throws `std::bad_variant_access` if the node isn't actually of type `T`, which is the correct behavior: a mismatch here is always an interpreter bug, not a user error, and a hard crash with a clear exception is preferable to silently reading the wrong field.

---

## How the interpreter uses this

The interpreter's `evaluate()` and `execute()` methods call `std::visit` on each `ASTNode`:

```cpp
QuantumValue Interpreter::evaluate(ASTNode& node) {
    return std::visit(overloaded{
        [&](BinaryExpr& e)  { return evalBinary(e); },
        [&](CallExpr& e)    { return evalCall(e); },
        [&](Identifier& e)  { return evalIdentifier(e); },
        // ... all other expression types
    }, node.node);
}
```

This dispatch is zero-overhead at runtime: the compiler generates a jump table indexed by the variant's type index, equivalent to a switch statement over an enum.