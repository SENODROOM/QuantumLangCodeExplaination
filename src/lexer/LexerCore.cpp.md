# LexerCore.cpp Summary

## Role in Compiler Pipeline

The `LexerCore.cpp` file is a crucial component of the Quantum Language compiler's pipeline. Its primary responsibility is to convert the source code into a sequence of tokens, which serve as the foundation for further processing by the parser to generate an abstract syntax tree (AST). The lexer ensures that the source code is accurately parsed and that any errors or invalid characters are properly flagged.

## Key Design Decisions and Why

### Tokenization Rules

The lexer uses a set of rules to identify different types of tokens such as identifiers, literals, operators, and keywords. These rules are implemented using functions like `current()`, `peek(int offset)`, and `advance()` to navigate through the source code efficiently.

### Handling Whitespace and Comments

To maintain readability and avoid unnecessary complexity during parsing, the lexer includes functionality to skip whitespace and comments. This is achieved with `skipWhitespace()`, `skipComment()`, and `skipBlockComment()` methods, ensuring that these elements do not interfere with the tokenization process.

### Error Handling

Effective error handling is essential for identifying issues in the source code early. The lexer throws exceptions when encountering unrecognized characters or malformed tokens, providing clear feedback on what went wrong and where.

## Major Classes/Functions Overview

### Lexer Class

- **Constructor**: Initializes the lexer with the source code string.
- **current()**: Returns the character at the current position.
- **peek(int offset)**: Returns the character at the specified offset relative to the current position.
- **advance()**: Advances the current position to the next character and updates line and column numbers accordingly.
- **skipWhitespace()**: Skips all whitespace characters until a non-whitespace character is encountered.
- **skipComment()**: Skips single-line comments starting with `//`.
- **skipBlockComment()**: Skips multi-line comments enclosed within `/* */`.

### Keywords Map

A constant unordered map named `keywords` stores common quantum language keywords and their corresponding token types. This allows for quick lookup and classification of keyword tokens during the lexing phase.

## Tradeoffs

### Complexity vs. Readability

While the lexer provides robust functionality for tokenizing complex source code, maintaining high readability and simplicity is also important. By carefully designing the tokenization rules and implementing straightforward logic, the lexer achieves a balance between performance and ease of maintenance.

### Error Reporting Precision

The lexer aims to provide precise error reporting by throwing exceptions with detailed information about the error location and nature. However, this requires additional overhead during the lexing process, which can impact performance slightly. Balancing thorough error reporting with efficient execution is a key challenge addressed in the design.

### Flexibility vs. Consistency

The lexer supports various programming styles and conventions, including those found in C++, JavaScript, and Python. While this flexibility enhances the compiler's usability across different languages, it also increases the complexity of the implementation. Ensuring consistent behavior across diverse languages while accommodating unique features remains a significant design consideration.