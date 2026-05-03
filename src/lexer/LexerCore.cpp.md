# LexerCore.cpp Summary

## Role in Compiler Pipeline

The `LexerCore.cpp` file is a critical component of the Quantum Language compiler's pipeline. It is responsible for converting the source code into a sequence of tokens, which are then processed by the parser to generate an abstract syntax tree (AST). The lexer plays a vital role in the compilation process as it breaks down the input text into meaningful units that can be easily parsed and interpreted by subsequent stages of the compiler.

## Key Design Decisions and Why

### Tokenization Rules

- **Character Classification**: The lexer uses character classification functions (`std::isdigit`, `std::isalpha`, etc.) to determine the type of each token.
- **Keyword Recognition**: A predefined set of keywords is used to recognize reserved words in the language. This allows the lexer to differentiate between identifiers and keywords, ensuring correct parsing.
- **String Handling**: Strings are handled with special consideration to escape sequences and delimiters. The lexer ensures that strings are correctly terminated and that escape characters are properly interpreted.
- **Comments**: Both single-line (`//`) and multi-line (`/* */`) comments are supported. Single-line comments extend until the end of the line, while multi-line comments span multiple lines until they are closed.

### Error Handling

- **Exception Propagation**: The lexer throws exceptions when encountering errors such as unterminated strings or invalid characters. These exceptions are caught by higher-level components of the compiler, allowing them to handle errors gracefully.
- **Detailed Error Messages**: When an error occurs, the lexer provides detailed messages indicating the position in the source code where the error was found. This helps developers quickly identify and fix issues in their code.

## Major Classes/Functions Overview

### Lexer Class

- **Constructor**: Initializes the lexer with the source code string.
- **Tokenize Method**: Main method that processes the source code and returns a sequence of tokens.
- **Current Character Method**: Returns the current character being processed.
- **Peek Method**: Allows looking ahead at the next character(s) without advancing the position.
- **Advance Method**: Advances the position in the source code and updates line and column information.
- **Skip Whitespace Method**: Skips over whitespace characters to ensure tokens are not separated by spaces.
- **Skip Comment Method**: Skips over single-line comments starting with `//`.
- **Skip Block Comment Method**: Skips over multi-line comments enclosed within `/* */`.

### Token Class

- Represents individual tokens in the source code.
- Contains properties such as type, value, line number, and column number.

### TokenType Enum

- Defines all possible types of tokens that can be produced by the lexer.
- Includes both keywords and other token categories like identifiers, literals, and operators.

## Tradeoffs

- **Performance vs. Flexibility**: While the lexer is designed to handle various edge cases (such as escape sequences and multi-line comments), this flexibility comes at the cost of performance. More complex tokenization rules require additional checks and processing steps.
- **Readability vs. Complexity**: The use of detailed error messages and comprehensive keyword recognition enhances readability but may increase the complexity of the lexer implementation.
- **Memory Usage**: Storing the entire source code in memory during lexing can lead to high memory usage, especially for large files. However, this approach simplifies the lexer's logic and avoids potential issues related to file I/O operations.

Overall, the `LexerCore.cpp` file is essential for the Quantum Language compiler, providing a robust foundation for tokenizing source code. Its design balances functionality, performance, and readability, making it well-suited for handling diverse programming constructs and edge cases.