# Lexer.h - The Heart of Lexical Analysis in Quantum Language Compiler

## Introduction

`Lexer.h` is a critical header file in the Quantum Language compiler, responsible for converting raw source code into a sequence of tokens. This process is known as lexical analysis or tokenization. The `Lexer` class plays a pivotal role in the compiler's pipeline, serving as the first stage where the syntax and semantics of the source code are understood.

## Key Design Decisions

### Use of `std::unordered_map` for Keywords and Macros

**Why:** Using an unordered map allows for constant-time average complexity for lookups, which is essential for quickly identifying keywords and expanding macros during the lexing phase. This choice enhances performance and scalability, especially when dealing with large codebases.

### Separate Handling for F-Strings

**Why:** F-strings (formatted string literals) require special handling to expand embedded expressions into their corresponding values. By maintaining a separate vector `pendingTokens_`, the lexer can manage these expansions efficiently without cluttering the main token stream.

### Support for C Preprocessor Directives

**Why:** Integrating support for C preprocessor directives (`#define`) enables the lexer to handle macro definitions and substitutions, making it more versatile and capable of processing complex source files. This feature is crucial for supporting conditional compilation and other preprocessing functionalities.

## Class Documentation

### Lexer Class

**Purpose:** The `Lexer` class is designed to take a source code string and convert it into a vector of tokens. It handles various lexical elements such as numbers, strings, identifiers, operators, and comments.

**Behavior:** Upon instantiation, the `Lexer` takes a reference to the source code string. The `tokenize()` method processes the entire source code, generating a vector of tokens. Each token represents a meaningful unit of the language, such as a keyword, identifier, or operator.

### Private Members

- **src**: Stores the source code string.
- **pos**: Tracks the current position within the source code.
- **line** and **col**: Maintain the current line and column numbers for error reporting and debugging purposes.
- **keywords**: A static unordered map that maps keywords to their corresponding token types.
- **pendingTokens_**: A vector used for managing f-string expansions.
- **defines_**: An unordered map that stores macro names and their replacement token lists.

### Methods

#### `current() const`
**Purpose:** Returns the character at the current position in the source code.
**Behavior:** Simply returns the character pointed to by `pos`.

#### `peek(int offset = 1) const`
**Purpose:** Allows lookahead at characters ahead of the current position.
**Behavior:** Returns the character at the position `pos + offset`. If `offset` is not provided, it defaults to 1.

#### `advance()`
**Purpose:** Advances the lexer to the next character in the source code.
**Behavior:** Increments `pos` and updates `line` and `col` accordingly. Returns the advanced character.

#### `skipWhitespace()`
**Purpose:** Skips over any whitespace characters in the source code.
**Behavior:** Continuously advances the lexer until a non-whitespace character is encountered.

#### `skipComment()`
**Purpose:** Skips over single-line comments in the source code.
**Behavior:** Advances the lexer past the comment marker (`//`) and all subsequent characters on the same line.

#### `skipBlockComment()`
**Purpose:** Skips over multi-line comments in the source code.
**Behavior:** Advances the lexer past the block comment markers (`/*` and `*/`) and all enclosed text.

#### `readNumber()`
**Purpose:** Reads and converts number literals from the source code.
**Behavior:** Identifies and parses numeric literals, including integers and floating-point numbers, returning them as a `Token`.

#### `readString(char quote)`
**Purpose:** Reads and converts string literals from the source code.
**Behavior:** Identifies and parses string literals enclosed in matching quotes, returning them as a `Token`.

#### `readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)`
**Purpose:** Handles the expansion of template literals (f-strings).
**Behavior:** Recursively reads and expands embedded expressions within a string literal, storing the resulting tokens in `out`.

#### `readIdentifierOrKeyword()`
**Purpose:** Reads and identifies variable names or keywords.
**Behavior:** Determines if the current sequence of characters forms an identifier or a keyword, returning the appropriate `Token`.

#### `readOperator()`
**Purpose:** Reads and identifies operator symbols.
**Behavior:** Recognizes and categorizes operator symbols, returning them as a `Token`.

## Tradeoffs and Limitations

- **Performance:** While the use of unordered maps improves lookup times, the overhead of managing macro expansions and f-string literals can impact performance, particularly in larger codebases.
- **Complexity:** Supporting both C-style preprocessor directives and f-string literals adds complexity to the lexer implementation, requiring careful handling of state transitions and nested structures.
- **Error Handling:** The lexer provides basic error handling through line and column information but may need enhancements to provide more detailed diagnostics for complex scenarios.

This README provides a comprehensive overview of the `Lexer.h` file, detailing its functionality, design choices, and potential limitations. For further details, refer to the accompanying implementation and test files.