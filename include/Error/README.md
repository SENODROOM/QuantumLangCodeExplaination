# Error.h — Error Handling & Terminal Colors

This header defines the exception hierarchy used throughout the Quantum Language runtime and a namespace of ANSI terminal color codes used when printing error messages.

---

## Exception hierarchy

```
std::runtime_error
    └── QuantumError          (base: adds line number + error kind)
            ├── RuntimeError  (execution failures: division by zero, stack overflow, etc.)
            ├── TypeError     (wrong type for an operation)
            ├── NameError     (reference to an undefined variable)
            └── IndexError    (out-of-bounds array/string access)
```

All Quantum exceptions inherit from `std::runtime_error`, which means they can be caught with a standard `catch (const std::exception& e)` block anywhere in the C++ host — useful for the REPL and test runner, which need to recover from user errors without crashing the process.

### `QuantumError` — the base class

```cpp
class QuantumError : public std::runtime_error {
public:
    int line;
    std::string kind;

    QuantumError(const std::string& kind, const std::string& msg, int line = -1)
        : std::runtime_error(msg), line(line), kind(kind) {}
};
```

Two fields are added on top of `std::runtime_error`:

- **`line`** — the source line where the error originated. Defaults to `-1` when the error is not traceable to a specific line (e.g., internal errors during initialization). The interpreter stores line numbers in every `ASTNode`, so most runtime errors can fill this in precisely.
- **`kind`** — a human-readable error category string (`"TypeError"`, `"NameError"`, etc.). This is separate from the C++ type so that error messages can be formatted as `TypeError: cannot add int and string` without the caller needing to `dynamic_cast` to determine the error category.

### Derived error types

Each subclass exists to distinguish error categories at the C++ type level, which allows the interpreter to catch specific error kinds selectively:

```cpp
try {
    result = evalBinary(expr);
} catch (const TypeError& e) {
    // report type mismatch with a specific message
} catch (const QuantumError& e) {
    // catch-all for other Quantum errors
}
```

The derived constructors simply forward their arguments to `QuantumError` with the appropriate `kind` string hardcoded:

```cpp
class RuntimeError : public QuantumError {
public:
    RuntimeError(const std::string& msg, int line = -1)
        : QuantumError("RuntimeError", msg, line) {}
};
```

This pattern means throw sites are concise — `throw TypeError("cannot add int and string", node.line)` — while the kind string is never accidentally misspelled, because it is set once in the constructor.

---

## The `Colors` namespace

```cpp
namespace Colors {
    inline const char* RED     = "\033[31m";
    inline const char* YELLOW  = "\033[33m";
    inline const char* GREEN   = "\033[32m";
    inline const char* CYAN    = "\033[36m";
    inline const char* BLUE    = "\033[34m";
    inline const char* WHITE   = "\033[37m";
    inline const char* MAGENTA = "\033[35m";
    inline const char* BOLD    = "\033[1m";
    inline const char* RESET   = "\033[0m";
}
```

These are ANSI SGR (Select Graphic Rendition) escape sequences. When written to a terminal that supports ANSI codes, they change the foreground color or style of subsequent text until `RESET` is written.

**Why `inline` variables?** Before C++17, defining variables in a header included by multiple translation units caused linker errors ("multiple definition"). The `inline` specifier (C++17) tells the linker to merge all definitions into one, allowing this header to be included freely without a separate `.cpp` file for the color constants.

**Usage pattern** in the error reporter:
```cpp
std::cerr << Colors::BOLD << Colors::RED
          << e.kind << ": "
          << Colors::RESET << e.what()
          << " (line " << e.line << ")\n";
```

The error type is printed in bold red, then `RESET` restores normal formatting before the message text, giving the user a visually clear signal about what kind of error occurred without making the entire output hard to read.

---

## Design note: what's missing

This hierarchy intentionally omits a `SyntaxError` type. Parse errors are reported and handled differently — they are detected during parsing before an `ASTNode` even exists, so there is no meaningful `line` field from a node. Parse errors are currently surfaced as thrown strings or `std::runtime_error` directly from the parser rather than going through this hierarchy. A future improvement would be to add `SyntaxError` here and route parser errors through it, giving parse failures the same structured reporting as runtime errors.