# Token.h — Token Types and the Token Structure

Tokens are the currency of the compiler pipeline. The lexer produces them; the parser consumes them. This header defines what a token is and what kinds exist.

---

## The `Token` struct

```cpp
struct Token {
    TokenType type;
    std::string value;
    int line;
    int col;

    Token(TokenType t, std::string v, int ln, int c)
        : type(t), value(std::move(v)), line(ln), col(c) {}

    std::string toString() const;
};
```

Every token carries four pieces of information:

- **`type`** — what kind of token this is (a number, an operator, a keyword, etc.)
- **`value`** — the raw text of the token as it appeared in the source. For a number token, this is the numeric string (`"42"`, `"3.14"`, `"0xFF"`). For a keyword, it's the keyword word (`"if"`, `"while"`). For operators and delimiters, it's the operator text (`"+"`, `"=="`). The parser uses `value` when it needs the actual content, not just the category.
- **`line`**, **`col`** — the source position of the token's first character. These propagate into `ASTNode::line` during parsing and ultimately into `QuantumError::line` when a runtime error is thrown. Tracking both line and column allows error messages to point to the exact character.

The constructor takes `value` by value and moves it, avoiding an extra copy when the lexer constructs tokens from temporary strings.

`toString()` is implemented in `Token.cpp` and produces a human-readable representation used in debug output and error messages.

---

## `enum class TokenType`

Using `enum class` rather than a plain `enum` prevents accidental comparisons between token types and integers, and avoids name collisions with identifiers in the rest of the codebase (a plain `enum` would pollute the global namespace with names like `PLUS` and `IF`).

### Literals

| Token | Represents |
|-------|-----------|
| `NUMBER` | Any numeric literal: integer, float, or hex |
| `STRING` | A `"..."` or `'...'` quoted string with escapes resolved |
| `TEMPLATE_STRING` | One text segment of a backtick template literal (the part before `${`) |
| `BOOL_TRUE` / `BOOL_FALSE` | The keywords `true` and `false` |
| `NIL` | The keyword `nil` |

### Identifiers and keywords

The token type distinguishes between user-defined identifiers (`IDENTIFIER`) and reserved words. This means the parser never needs to check whether an identifier is reserved — if a token arrives as `IDENTIFIER`, it's guaranteed to be a user-defined name.

**Function declaration keywords** — three aliases for the same concept:
- `FN` — Quantum's native syntax (`fn foo() { }`)
- `DEF` — Python syntax (`def foo(): `)
- `FUNCTION` — JavaScript syntax (`function foo() { }`)

The parser accepts all three interchangeably.

**Class and object keywords** — `CLASS`, `EXTENDS`, `NEW`, `THIS`, `SUPER` provide a JavaScript/Python-style OOP vocabulary. `THIS` is an alias for `self`, matching JavaScript convention.

**Control flow** — `IF`, `ELSE`, `ELIF`, `WHILE`, `FOR`, `IN`, `OF`, `BREAK`, `CONTINUE`, `RETURN`. The `OF` token specifically supports JavaScript's `for...of` loop syntax as an alternative to `for...in`.

**Exception handling** — `TRY`, `EXCEPT`, `FINALLY`, `RAISE`, `AS`. Python's exception vocabulary rather than C++'s `try`/`catch`/`throw`.

**I/O keywords** — `PRINT`, `INPUT` are first-class statement keywords rather than library functions. `COUT` and `CIN` are also keywords, allowing C++-style `cout << x` and `cin >> x` syntax.

**C/C++ type keywords** — `TYPE_INT`, `TYPE_FLOAT`, `TYPE_DOUBLE`, `TYPE_CHAR`, `TYPE_STRING`, `TYPE_BOOL`, `TYPE_VOID`, `TYPE_LONG`, `TYPE_SHORT`, `TYPE_UNSIGNED`. These allow C-style variable declarations (`int x = 5`, `float* p = ...`) and are checked by `isCTypeKeyword()` in the parser.

**Cybersecurity reserved keywords** — `SCAN`, `PAYLOAD`, `ENCRYPT`, `DECRYPT`, `HASH`. These are reserved but not yet connected to any language construct. Reserving them now prevents user code from using them as variable names, making a future implementation non-breaking.

### Operators

The operator set covers three overlapping syntactic traditions:

**Arithmetic** — `PLUS`, `MINUS`, `STAR`, `SLASH`, `PERCENT`, `POWER` (`**`), `FLOOR_DIV` (`//` integer division from Python).

**Comparison** — `EQ` (`==`), `NEQ` (`!=`), `STRICT_EQ` (`===`), `STRICT_NEQ` (`!==`), `LT`, `GT`, `LTE`, `GTE`. The strict equality operators mirror JavaScript — they distinguish value equality from identity equality.

**Logical** — `AND`, `OR`, `NOT` (Python-style keyword operators), plus `AND_AND` (`&&`) and `OR_OR` (`||`) (C/JavaScript-style symbol operators). Both spellings are valid; the parser treats them identically.

**Bitwise** — `BIT_AND` (`&`), `BIT_OR` (`|`), `BIT_XOR` (`^`), `BIT_NOT` (`~`), `LSHIFT` (`<<`), `RSHIFT` (`>>`).

**Assignment** — `ASSIGN` (`=`), `PLUS_ASSIGN` (`+=`), `MINUS_ASSIGN` (`-=`), `STAR_ASSIGN` (`*=`), `SLASH_ASSIGN` (`/=`), `AND_ASSIGN` (`&=`), `OR_ASSIGN` (`|=`), `XOR_ASSIGN` (`^=`), `MOD_ASSIGN` (`%=`). All compound assignments are first-class tokens, not synthesized by the parser.

**Other** — `FAT_ARROW` (`=>`), used for arrow function syntax. `PLUS_PLUS` (`++`) and `MINUS_MINUS` (`--`). `NULL_COALESCE` (`??`) from JavaScript. `IS` for identity comparison.

### Delimiters

`LPAREN`, `RPAREN`, `LBRACE`, `RBRACE`, `LBRACKET`, `RBRACKET`, `COMMA`, `SEMICOLON`, `COLON`, `DOT`, `ARROW` (`->`), `QUESTION`, `DECORATOR` (`@`), `NEWLINE`.

`NEWLINE` is emitted by the lexer but currently not used as a statement terminator — Quantum uses braces, not significant newlines, for block structure. The token exists because some error recovery logic uses newlines as sync points.

### Special tokens

- `EOF_TOKEN` — always the last token in the vector. The parser checks `atEnd()` by looking for this token, which avoids out-of-bounds vector access.
- `UNKNOWN` — emitted for unrecognized characters. The parser treats any `UNKNOWN` token as a syntax error, producing a specific error message rather than a crash.
- `INDENT` / `DEDENT` — reserved for Python-style significant indentation. Currently not emitted; the lexer uses braces for block structure. These exist to support a future indentation-sensitive parsing mode.

---

## Why `value` is always a string

Every token stores its text as a `string`, even for numbers. This means the lexer never needs to decide between `int` and `float` parsing — that decision belongs to the parser, which has enough context to know whether an integer or floating-point value is expected. It also means numeric formatting (hex vs decimal vs scientific notation) is preserved if the code is ever pretty-printed or error-reported.

The cost is a heap allocation per string token. For typical source files this is negligible; if profiling ever identified token construction as a bottleneck, `std::string_view` into the original source would be an option, at the cost of tying token lifetime to source string lifetime.