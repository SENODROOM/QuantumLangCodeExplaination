# Quantum Language — Compiler Architecture

Quantum is a dynamically-typed interpreted language implemented in C++. It borrows syntax from Python, JavaScript, and C++, targeting cybersecurity scripting with a built-in standard library of cryptographic and network primitives. The runtime is a classic tree-walking interpreter with an optional static type checker.

This document explains how the components fit together, the key design decisions, and the tradeoffs involved.

---

## Pipeline overview

Source text travels through five stages before producing output:

```
Source (.sa file)
      │
      ▼
   Lexer                    → vector<Token>
      │
      ▼
   Parser                   → vector<ASTNode>
      │
      ▼
   TypeChecker (optional)   → errors or pass
      │
      ▼
   Interpreter              → side effects / return value
```

Each stage is a separate class with a well-defined input and output type. The stages do not share mutable state: the lexer owns the source string, the parser owns the token vector, and the interpreter owns the environment. This separation means any stage can be tested independently by constructing its input directly.

---

## Lexer

The lexer converts raw source text into a flat sequence of tokens in a single forward pass — O(n) in the length of the source. It never backtracks.

```cpp
class Lexer {
public:
    explicit Lexer(const std::string& source);
    std::vector<Token> tokenize();
private:
    std::string src_;
    size_t pos_ = 0;
    int line_ = 1;
    ...
};
```

The key private methods and what they handle:

| Method | Responsibility |
|--------|---------------|
| `advance()` | Consume one character, updating `line_` on `\n` |
| `peek()` | Look at the next character without consuming it |
| `readIdentifierOrKeyword()` | Reads an alphanumeric sequence; checks against a `std::unordered_map` for keywords |
| `readNumber()` | Handles integer, floating-point, and hex literals |
| `readString()` | Handles `"..."` and `'...'` with escape sequences |
| `readTemplateLiteral()` | Handles backtick strings with `${expr}` interpolation |
| `skipComment()` | Skips `//` line comments |
| `skipBlockComment()` | Skips `/* ... */` block comments |
| `skipWhitespace()` | Advances past spaces, tabs, and newlines |

Keyword lookup uses an `unordered_map<string, TokenType>` populated once at construction. This gives O(1) lookup per identifier, which matters because identifier tokens are the most common token type in typical source files.

Template literals deserve special mention: when the lexer encounters `` ` ``, it enters a mode where it alternates between reading literal string segments and tokenizing `${...}` interpolation expressions. The interpolation content is fully tokenized inline — the lexer does not defer it to the parser.

---

## Tokens

```cpp
enum class TokenType {
    // Literals
    Number, String, Bool, Nil,
    // Identifiers and keywords
    Identifier, Let, Const, Fn, Return, If, Elif, Else,
    While, For, In, Break, Continue, Class, New, Import, From, As,
    Print, Input, And, Or, Not, True, False, Try, Except, Raise, Finally,
    // Operators and punctuation
    Plus, Minus, Star, Slash, Percent, EqualEqual, BangEqual,
    Less, LessEqual, Greater, GreaterEqual, Equal, PlusEqual, ...
    // Pointer operators
    Ampersand, Arrow,
    // EOF
    Eof
};

struct Token {
    TokenType type;
    std::string value;
    int line;
};
```

Every token carries the source line number. This is propagated into `ASTNode::line` during parsing, which is then used by the interpreter and error reporter. If you follow an error message back to its source, the line number chain is: `QuantumError::line` ← `ASTNode::line` ← `Token::line` ← `Lexer::line_`.

---

## Parser

The parser is a hand-written recursive descent parser with Pratt parsing for expressions. It consumes a `vector<Token>` and produces a `vector<ASTNodePtr>` (the top-level statement list).

Pratt parsing handles operator precedence without a grammar table. Each token type is assigned a binding power, and `parseExpr(minBP)` calls itself recursively only when the next operator has a higher binding power than `minBP`. This naturally produces the correct precedence and associativity without separate grammar rules for every precedence level.

```
Precedence (low → high):
  Assignment (=, +=, ...)
  Ternary (a if cond else b)
  Or / And
  Equality (==, !=)
  Comparison (<, >, <=, >=)
  Addition / Subtraction
  Multiplication / Division / Modulo
  Unary (-, !, ~, &, *)
  Postfix (calls, indexing, member access, ->)
```

The parser is also responsible for validating syntax that the grammar alone cannot express, such as ensuring that `break` and `continue` only appear inside loops, and that `return` only appears inside a function body. These checks are done by tracking a nesting depth counter, not by a separate pass.

### Key parsing methods

| Method | Handles |
|--------|---------|
| `parse()` | Top-level entry point; parses until EOF |
| `parseStatement()` | Dispatches to the right parse method based on the current token |
| `parseExpr(minBP)` | Pratt expression parser |
| `parseAssignment()` | Assignment targets, including complex lvalues |
| `parsePrimary()` | Literals, identifiers, grouped expressions, array/dict literals, lambdas |
| `parseFunctionDecl()` | Function signature with default args and reference params (`int& r`) |
| `parseVarDecl()` | `let`/`const` with optional type annotation |

---

## Abstract Syntax Tree

The AST uses `std::variant` rather than virtual dispatch. All node types are members of `NodeVariant`, and every tree node is an `ASTNode` wrapping a `NodeVariant` and a line number.

```cpp
struct ASTNode {
    NodeVariant node;
    int line = 0;
    template <typename T> T& as() { return std::get<T>(node); }
    template <typename T> bool is() const { return std::holds_alternative<T>(node); }
};
```

The variant design has two important properties for this use case:

1. **Exhaustive visitor enforcement.** Adding a new node type without handling it in `std::visit` is a compile error, not a runtime bug.
2. **Inline storage.** Node data is stored directly in `ASTNode` rather than behind an extra pointer, reducing allocations and improving cache behavior when the interpreter traverses the tree.

Child nodes are owned via `unique_ptr<ASTNode>` (`ASTNodePtr`), giving each node exclusive ownership of its subtree. This makes the tree trivially destructible — dropping the root drops the whole tree.

---

## Type Checker

The type checker is an optional pass that runs between parsing and interpretation. It walks the AST and infers types for expressions, flagging mismatches before the program runs.

```cpp
class TypeChecker {
public:
    void check(const std::vector<ASTNodePtr>& program);
private:
    std::unordered_map<std::string, std::string> typeEnv_;
    std::string inferType(const ASTNode& node);
    void checkStatement(const ASTNode& node);
};
```

The type system is intentionally shallow — it catches obvious errors like assigning a string to a variable declared as `int`, but it does not attempt full Hindley-Milner inference. The type environment maps variable names to their declared types. When a variable is declared without a type annotation, its type is inferred from its initializer and recorded. Subsequent assignments that would change the inferred type are flagged as `StaticTypeError`.

Because the language is dynamically typed at runtime, type-checking errors from this pass are currently reported as warnings rather than hard failures. This allows gradual adoption: users can annotate the parts of their code they want checked without being forced to annotate everything.

---

## Interpreter

The interpreter is a tree-walking evaluator. It maintains an `Environment` (a linked chain of scopes) and walks the AST by calling `execute()` for statements and `evaluate()` for expressions.

### Environment and scoping

```cpp
class Environment {
public:
    std::shared_ptr<Environment> parent;
    std::unordered_map<std::string, QuantumValue> vars;

    void define(const std::string& name, QuantumValue val);
    QuantumValue& get(const std::string& name);
    void set(const std::string& name, QuantumValue val);
};
```

`get()` walks the `parent` chain until it finds the variable. `set()` also walks the chain — it updates the innermost scope that already defines the variable, which implements Python-style closure semantics. `define()` always writes to the current scope without walking up, which is how `let` creates a new binding even when the same name exists in an outer scope.

When a function is called, a new `Environment` is created with its `parent` pointing to the environment where the function was **defined** (not where it was called). This is lexical scoping, which means closures work correctly:

```
fn makeCounter() {
    let n = 0
    return fn() { n += 1; return n }
}
let c = makeCounter()
c()  // 1
c()  // 2
```

The inner function's environment captures the outer function's `n`, and that capture persists across calls.

### Control flow signals

`return`, `break`, and `continue` are implemented as C++ exceptions:

```cpp
struct ReturnSignal { QuantumValue value; };
struct BreakSignal {};
struct ContinueSignal {};
```

When `execReturn` is called, it throws `ReturnSignal`. `callFunction` catches it and extracts the return value. `execWhile` and `execFor` catch `BreakSignal` and `ContinueSignal`. This design avoids threading a "did-we-return?" flag through every method and keeps the call stack clean.

### Infinite-loop protection

```cpp
long long stepCount_ = 0;
static constexpr long long MAX_STEPS = 2'000'000;
```

Every statement execution increments `stepCount_`. When the limit is exceeded, a `RuntimeError` is thrown. This is primarily for the test runner, which provides no stdin: a program waiting for input in a loop would otherwise hang the test process. The 2M step limit is generous enough that legitimate programs never hit it in normal use.

### Value system

All runtime values are `QuantumValue`, a `std::variant` over the possible runtime types:

```cpp
using QuantumValue = std::variant<
    double,                           // All numbers (int and float)
    bool,
    std::string,
    std::nullptr_t,                   // nil
    std::shared_ptr<Array>,           // mutable arrays
    std::shared_ptr<Dict>,            // mutable dicts
    std::shared_ptr<QuantumFunction>, // user-defined functions (closures)
    std::shared_ptr<QuantumNative>,   // C++ native functions
    std::shared_ptr<QuantumClass>,    // class definitions
    std::shared_ptr<QuantumInstance>, // class instances
    std::shared_ptr<QuantumPointer>   // reference/pointer values
>;
```

Numbers are uniformly `double`. This simplifies the value system at the cost of precision for very large integers (>2^53). Most cybersecurity scripts deal with byte arrays and hash values rather than big-integer arithmetic, so this tradeoff is reasonable.

`Array` and `Dict` are heap-allocated and reference-counted (`shared_ptr`) because they can be shared: assigning an array to a variable does not copy it, it copies the `shared_ptr`. This matches Python's semantics and avoids expensive copies when arrays are passed to functions.

### Native function registration

Built-in functions are registered in `registerNatives()` by binding C++ lambdas to names in the global environment:

```cpp
void Interpreter::registerNatives() {
    env->define("len", QuantumValue(std::make_shared<QuantumNative>(
        [](std::vector<QuantumValue> args) -> QuantumValue {
            // validate args, return length
        }
    )));
}
```

Adding a new built-in requires only adding a new `define()` call here and writing the lambda. No parser or AST changes are needed.

---

## Standard library categories

| Category | Examples | Notes |
|----------|---------|-------|
| Math | `sin`, `cos`, `sqrt`, `log`, `floor`, `ceil` | Thin wrappers over `<cmath>` |
| Strings | `len`, `substr`, `replace`, `split`, `trim` | Implemented as both free functions and instance methods |
| Collections | `push`, `pop`, `sort`, `filter`, `map`, `keys` | Dispatched via `callArrayMethod` / `callDictMethod` |
| I/O | `read_file`, `write_file`, `append_file` | Uses `std::filesystem` for path handling |
| Time | `now`, `sleep`, `format_time` | Millisecond precision via `<chrono>` |
| Random | `rand`, `srand` | Seeded with `std::mt19937` |
| Cybersecurity | `sha256`, `aes128`, `scan_port`, `hex_encode` | The domain-specific differentiator |
| Type | `typeof`, `is_number`, `is_string`, `to_int` | Runtime type introspection and conversion |

---

## Error reporting

The error reporter receives a `QuantumError`, the original source string, and the error's line number. It:

1. Prints the error kind and message in bold red.
2. Extracts the relevant source line from the source string and prints it.
3. Prints a caret (`^`) under the column where the error occurred.

```
TypeError: cannot add 'int' and 'string' (line 14)
    let result = count + " items"
                 ^^^^^
```

This requires that every `ASTNode` carries its source line (set during parsing from `Token::line`), and that the interpreter passes `node.line` to every error it throws.

---

## CLI and REPL

The `main.cpp` entry point supports three modes:

- **File mode** (`quantum script.sa`): reads, parses, optionally type-checks, and executes a file.
- **REPL mode** (`quantum` with no arguments): runs an interactive read-eval-print loop. Each input line is lexed, parsed, and executed independently. A `StreamGuard` object restores `std::cin` between iterations so that a program that reads input doesn't corrupt the REPL's stdin.
- **Test mode** (`quantum --test dir/`): discovers all `.sa` files under a directory, runs each with a non-blocking stdin (input returns `""` immediately), and reports pass/fail based on expected output annotations in the source file.

---

## Adding a new language feature

Every new feature touches exactly the same five layers in the same order:

1. **Lexer** — add a `TokenType` enum value and a case in the tokenizer.
2. **Parser** — add a `parse*` method or case in `parseStatement()`/`parsePrimary()`.
3. **AST** — add a struct for the new node, add it to `NodeVariant`.
4. **TypeChecker** — add inference and checking rules for the new construct.
5. **Interpreter** — add an `exec*` or `eval*` method, add a case to `execute()`/`evaluate()`.

This order is enforced by the compilation dependency chain: the interpreter includes `AST.h`, the parser produces `ASTNode` objects, and the lexer produces `Token` objects consumed by the parser. Skipping a layer produces a compile error, not a silent bug.

---

## Cross-platform considerations

### Windows
- `SetConsoleOutputCP(CP_UTF8)` is called at startup to enable UTF-8 output in the Windows console, which is required for ANSI color codes and Unicode source identifiers to display correctly.
- Path separators are handled via `std::filesystem::path`, which abstracts `\` vs `/` differences.
- Line endings: the lexer strips `\r` before processing, so `.sa` files with Windows line endings work correctly.

### Unix/Linux
- ANSI color codes work without any setup.
- `SIGINT` (Ctrl+C) is handled gracefully in the REPL to exit the current input without crashing.

---

## Known limitations and future work

| Limitation | Impact | Potential fix |
|-----------|--------|---------------|
| No JIT compilation | Programs with hot loops are slower than necessary | LLVM backend or bytecode VM |
| Shallow type checker | Does not catch all type errors before runtime | Full inference pass (Hindley-Milner) |
| No module caching | Importing the same module twice re-executes it | Cache parsed ASTs by file path |
| No debugger | Step-through debugging requires adding print statements | Language Server Protocol + DAP adapter |
| `double` for all numbers | Loss of precision for integers > 2^53 | Separate `int64` type in `QuantumValue` |
| No package manager | Distributing libraries requires manual file copying | Package registry + `quantum install` |