# QuantumLanguage Compiler - Lexer.h

## Overview

The `include/Lexer.h` header file is a crucial component of the QuantumLanguage compiler, dedicated to the lexical analysis phase. This phase involves breaking down the source code into individual tokens, which are then processed by the parser to generate an abstract syntax tree (AST). The `Lexer` class encapsulates the logic for reading the input source code and converting it into a sequence of tokens.

## Key Design Decisions

### Use of `std::variant` for Token Types

One of the primary design decisions was to use `std::variant` to represent different token types. This approach allows the lexer to encapsulate various value types within a single token structure, ensuring flexibility and reducing the overhead associated with multiple token classes. However, this choice comes with a limitation: `std::variant` does not support custom destructors or move semantics directly, which might require additional handling to manage resources properly.

### Handling F-Strings with Pending Tokens

F-strings (formatted string literals) are a feature that allows variables to be embedded within string literals using curly braces `{}`. To support this feature, the lexer uses a `pendingTokens_` vector to temporarily store tokens during the expansion process. This design decision ensures that the lexer can correctly parse and expand f-strings while maintaining the integrity of the original source code. However, it adds complexity to the lexer's implementation and may impact performance due to the need for additional token management.

### Preprocessor Macros Support

To enhance the functionality of the QuantumLanguage compiler, the lexer includes support for preprocessor macros. This feature allows users to define constants or complex token sequences that can be expanded throughout the source code. The `defines_` unordered map stores macro names as keys and their corresponding replacement token lists as values. While this feature significantly improves code reusability and maintainability, it also increases the complexity of the lexer and requires careful handling to avoid unintended side effects.

## Class and Function Documentation

### Lexer Class

#### Purpose

The `Lexer` class is responsible for reading the input source code and generating a sequence of tokens. It handles whitespace, comments, and various types of literals such as numbers, strings, and identifiers.

#### Behavior

- **Constructor**: Initializes the lexer with the provided source code.
- **tokenize()**: Reads the entire source code and returns a vector of tokens.

### Private Member Functions

#### current() const

- **Purpose**: Returns the character at the current position in the source code.
- **Behavior**: Returns the character at `pos`.

#### peek(int offset = 1) const

- **Purpose**: Returns the character at the specified offset relative to the current position.
- **Behavior**: Returns the character at `pos + offset`.

#### advance()

- **Purpose**: Advances the lexer to the next character in the source code.
- **Behavior**: Increments `pos` and returns the previous character.

#### skipWhitespace()

- **Purpose**: Skips any whitespace characters in the source code.
- **Behavior**: Continuously advances the lexer until a non-whitespace character is encountered.

#### skipComment()

- **Purpose**: Skips a single-line comment starting with `//`.
- **Behavior**: Continuously advances the lexer until the end of the line is reached.

#### skipBlockComment()

- **Purpose**: Skips a multi-line comment enclosed between `/*` and `*/`.
- **Behavior**: Continuously advances the lexer until the closing block comment is found.

#### readNumber()

- **Purpose**: Reads a numeric literal from the source code.
- **Behavior**: Parses consecutive digits and returns a `Token` representing the number.

#### readString(char quote)

- **Purpose**: Reads a string literal from the source code.
- **Behavior**: Parses characters until the matching quote is found and returns a `Token` representing the string.

#### readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)

- **Purpose**: Reads a template literal (f-string) from the source code.
- **Behavior**: Parses characters and expands embedded expressions, storing the resulting tokens in the output vector.

#### readIdentifierOrKeyword()

- **Purpose**: Reads an identifier or keyword from the source code.
- **Behavior**: Parses consecutive valid identifier characters and checks if the result is a keyword. If so, returns the corresponding `TokenType`; otherwise, returns an `IDENTIFIER` token.

#### readOperator()

- **Purpose**: Reads an operator from the source code.
- **Behavior**: Parses consecutive valid operator characters and returns a `Token` representing the operator.

## Tradeoffs and Limitations

- **Flexibility vs. Complexity**: Using `std::variant` for token types enhances flexibility but introduces complexity in managing token lifetimes and resource allocation.
- **Performance**: Supporting f-strings and preprocessor macros can impact performance due to increased token processing and memory usage.
- **Error Handling**: The lexer focuses primarily on tokenization and minimal error reporting. More sophisticated error handling mechanisms would require additional development effort.

This README.md provides a comprehensive overview of the `include/Lexer.h` header file, detailing its purpose, behavior, and design considerations. It also highlights the tradeoffs and limitations inherent in the implementation, offering insights into potential areas for improvement.