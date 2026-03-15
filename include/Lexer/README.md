# Lexer.h — The Lexical Analyser

The lexer is the first stage of the compiler pipeline. It takes the raw source string and produces a `vector<Token>`, consuming the input in a single left-to-right pass with no backtracking. Every subsequent stage — parser, type checker, interpreter — works entirely on tokens; none of them ever touch the source string directly.

---

## Public interface

```cpp
class Lexer {
public:
    explicit Lexer(const std::string& source);
    std::vector<Token> tokenize();
    ...
};
```

The constructor is `explicit` to prevent accidental implicit conversion from a string. `tokenize()` is the only public method: call it once, get back the complete token list. The lexer is not designed to be called incrementally.

---

## Private state

```cpp
private:
    std::string src;
    size_t pos;
    int line, col;

    static const std::unordered_map<std::string, TokenType> keywords;

    std::vector<Token> pendingTokens_;
    std::unordered_map<std::string, std::vector<Token>> defines_;
```

- **`src`, `pos`** — the source string and the current read position. All character-level operations index into `src` via `pos`.
- **`line`, `col`** — updated on every character consumed, so every emitted token carries a precise source location.
- **`keywords`** — a `static const` map, initialized once at program start and shared across all `Lexer` instances. Keyword lookup is O(1) per identifier, which matters because identifiers are the most frequently produced token type. The map is static because the keyword set is fixed for the lifetime of the program.
- **`pendingTokens_`** — a queue of tokens that have been produced but not yet returned by `tokenize()`. Used by template literal expansion: when the lexer encounters a `` ` `` string, it may generate multiple tokens (alternating string segments and interpolated-expression tokens) that need to be drained before the main loop continues.
- **`defines_`** — a map from macro name to replacement token list, populated by `#define` directives. When the lexer reads an identifier that matches a defined macro, it expands the macro inline. This gives the language basic C-preprocessor-style macro support without a separate preprocessing pass.

---

## Character-level primitives

```cpp
char current() const;       // src[pos], or '\0' at end
char peek(int offset=1) const; // src[pos+offset], or '\0'
char advance();             // return src[pos], then ++pos (updates line/col)
```

These three methods are the only way any other method reads from `src`. `current()` and `peek()` are non-consuming reads used for lookahead. `advance()` is the single point where `pos` moves forward — it also increments `col` and, on `\n`, resets `col` to 1 and increments `line`. Because all position tracking runs through `advance()`, it is impossible for `line`/`col` to get out of sync with `pos`.

---

## Whitespace and comment skipping

```cpp
void skipWhitespace();      // spaces, tabs, \r, \n
void skipComment();         // // ... to end of line
void skipBlockComment();    // /* ... */
```

`skipWhitespace()` calls `advance()` for each whitespace character. It does **not** emit `NEWLINE` tokens — Quantum Language does not use newlines as statement terminators. This is different from Python, where the lexer must track indentation to emit `INDENT`/`DEDENT` tokens.

`skipBlockComment()` calls `advance()` until it sees `*/`, handling the edge case of a block comment that spans multiple lines (which increments `line` correctly because all character consumption goes through `advance()`).

---

## Token readers

### `readNumber()`
Handles three numeric formats:
- Integer literals: `42`
- Floating-point literals: `3.14`, `1.0e-5`
- Hex literals: `0xFF`

Returns a `Token` with `type = NUMBER` and `value` holding the raw numeric string. Conversion to `double` happens in the parser, not the lexer, so the lexer never loses information about the original representation.

### `readString(char quote)`
Takes the opening quote character (`'` or `"`) and reads until the matching closing quote, handling escape sequences (`\n`, `\t`, `\\`, `\"`, etc.). Returns a `STRING` token with the escape sequences already resolved in the value — downstream code never needs to unescape.

### `readTemplateLiteral(vector<Token>& out, int startLine, int startCol)`
Template literals (backtick strings) are the most complex case. A literal like `` `Hello ${name}, you have ${count} messages` `` must produce:

```
TEMPLATE_STRING("Hello ")
IDENTIFIER(name)
TEMPLATE_STRING(", you have ")
IDENTIFIER(count)
TEMPLATE_STRING(" messages")
```

Rather than returning a single token, `readTemplateLiteral` writes directly into `out` (the pending token queue). When the main `tokenize()` loop encounters these pending tokens, it drains the queue before continuing with the next character. Interpolated expressions inside `${...}` are tokenized recursively by re-entering the main tokenization logic, which means nested template literals work correctly.

### `readIdentifierOrKeyword()`
Reads an alphanumeric+underscore sequence. After reading, checks `keywords` to determine whether it's a keyword token or a plain `IDENTIFIER`. If the resulting identifier matches an entry in `defines_`, it pushes the macro's replacement tokens onto `pendingTokens_` instead of emitting the identifier directly.

### `readOperator()`
Handles all multi-character operators by reading the first character, then peeking ahead to check for two- and three-character forms:
- `=` → check for `==`, `===`
- `!` → check for `!=`, `!==`
- `<` → check for `<=`, `<<`
- `>` → check for `>=`, `>>`
- `&` → check for `&&`, `&=`
- `|` → check for `||`, `|=`
- `-` → check for `->`, `--`, `-=`
- `?` → check for `??`

This is a classic maximal-munch strategy: always consume the longest valid operator.

---

## Macro support (`defines_`)

When the lexer encounters `#define NAME token1 token2 ...`, it tokenizes the replacement list and stores it under `NAME` in `defines_`. On any subsequent read of `NAME` as an identifier, the stored replacement tokens are pushed to `pendingTokens_` and drained on the next loop iterations. This is a token-level substitution, not a text-level substitution, so it cannot cause tokenization ambiguity.

This feature is deliberately limited — there is no support for function-like macros (`#define MAX(a,b) ...`) or `#ifdef`/`#endif` conditional compilation. Those would require a separate preprocessing pass with its own state machine.

---

## Design notes

**Why no separate preprocessing stage?** Many C-family compilers run the preprocessor as a completely separate tool that transforms source text before the lexer sees it. Integrating `#define` expansion directly into the lexer avoids the need for a separate text-transformation pass and keeps the tool chain to a single executable. The tradeoff is that macro expansion is limited to token-level substitution; text-level operations like `#` stringification and `##` token-pasting are not supported.

**Why `static const` for keywords?** A non-static map would be re-allocated and re-populated on every `Lexer` construction. Since the keyword set never changes, a static map is initialized once (at the first construction or at program start, depending on the platform's static initialization rules) and reused for every subsequent `Lexer` instance.