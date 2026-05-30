# LexerCore.cpp Summary

## Role in Compiler Pipeline

The `LexerCore.cpp` file is a crucial component of the Quantum Language compiler, serving as the initial stage of the compilation process. Its primary responsibility is to convert the source code into a sequence of tokens, which are then used by the parser and other subsequent stages to construct the abstract syntax tree (AST). This conversion ensures that the compiler can understand and process the input code accurately.

## Key Design Decisions and Why

### Tokenization Rules

The lexer follows specific rules to identify and categorize different elements of the Quantum Language. These rules include recognizing keywords, identifiers, literals, operators, and punctuation marks. By adhering to these rules, the lexer ensures that the source code is parsed correctly and efficiently.

### Handling Whitespace and Comments

To maintain readability and simplify the parsing process, the lexer skips whitespace characters (`' '`, `'\t'`, `'\r'`) and comments (`//`, `/* */`). This decision simplifies the task of the parser by removing unnecessary clutter, making it easier to focus on the structure and semantics of the code.

### Error Handling

The lexer includes basic error handling mechanisms to manage unexpected characters or malformed tokens. When an error is detected, it throws exceptions with appropriate error messages, facilitating debugging and ensuring that the compiler provides clear feedback to the user.

## Major Classes/Functions Overview

### Lexer Class

- **Purpose**: Manages the tokenization process for the Quantum Language source code.
- **Constructor**: Initializes the lexer with the source code string.
- **Member Functions**:
  - `current()`: Returns the character at the current position.
  - `peek(int offset)`: Returns the character at the specified offset relative to the current position.
  - `advance()`: Advances the lexer to the next character and updates line and column numbers accordingly.
  - `skipWhitespace()`: Skips all whitespace characters starting from the current position.
  - `skipComment()`: Skips single-line comments starting with `//`.
  - `skipBlockComment()`: Skips block comments enclosed within `/* */`.

### Keywords Map

- **Purpose**: Maps common quantum language keywords to their corresponding token types.
- **Why**: This map helps in quickly identifying and categorizing keywords during the tokenization process, reducing the complexity of the lexer.

## Tradeoffs

### Performance vs. Readability

While the lexer aims for simplicity and efficiency, there is a tradeoff between performance and readability. Some optimizations might reduce the clarity of the code, making it harder for developers to understand and maintain.

### Complexity of Tokenization Rules

The lexer's ability to handle various language elements adds complexity to its implementation. However, this complexity is necessary to ensure accurate tokenization and proper parsing of the source code.

### Error Reporting

Basic error reporting through exceptions is implemented, which might not be sufficient for complex languages. Improving error reporting could involve more sophisticated techniques, such as context-sensitive error messages or warnings.

By carefully balancing these considerations, the `LexerCore.cpp` file contributes significantly to the overall functionality and reliability of the Quantum Language compiler.