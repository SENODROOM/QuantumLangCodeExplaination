# LexerCore.cpp Summary

## Role in Compiler Pipeline

The `LexerCore.cpp` file is an essential part of the Quantum Language compiler, functioning as the initial phase of the compilation process. Its main task is to transform the source code into a series of tokens, which are subsequently utilized by the parser and other components to generate the abstract syntax tree (AST).

## Key Design Decisions and Why

1. **Tokenization Rules**: The lexer follows a set of predefined rules to identify different types of tokens such as identifiers, literals, operators, and keywords. These rules ensure that the source code is accurately parsed into meaningful units.

2. **State Machine Approach**: To efficiently handle complex token patterns, the lexer uses a state machine approach. This method allows for the transition between states based on input characters, enabling the recognition of multi-character tokens like strings or comments.

3. **Error Handling**: Robust error handling mechanisms are implemented within the lexer to manage unexpected characters or malformed tokens. These errors are propagated up the compiler pipeline, allowing for appropriate reporting and recovery strategies.

4. **Unicode Support**: The lexer supports Unicode characters, ensuring compatibility with international programming practices and diverse character sets.

5. **Line and Column Tracking**: Accurate tracking of line and column numbers helps in providing precise error messages and debugging information.

## Major Classes/Functions Overview

### Lexer Class

- **Constructor (`Lexer::Lexer(const std::string &source)`)**: Initializes the lexer with the source code string.
  
- **Member Functions**:
  - `current()`: Returns the current character being processed.
  - `peek(int offset)`: Returns the character at the specified offset ahead without advancing the position.
  - `advance()`: Advances the position to the next character and updates line and column numbers accordingly.
  - `skipWhitespace()`: Skips all whitespace characters until a non-whitespace character is encountered.
  - `skipComment()`: Skips single-line comments starting with `//`.
  - `skipBlockComment()`: Skips block comments enclosed by `/* */`.

### Tokenization Logic

- **Identifiers and Keywords**: The lexer recognizes identifiers and checks them against a predefined list of keywords. If a keyword is found, it returns the corresponding token type; otherwise, it identifies the identifier.

- **Literals**: Numeric literals, string literals, boolean values, and `nil` are recognized and returned as their respective token types.

- **Operators and Delimiters**: Various operators and delimiters are identified and returned as tokens.

- **Comments**: Both single-line and block comments are skipped during the tokenization process to avoid interference with the parsing stage.

## Tradeoffs

1. **Complexity vs. Performance**: Implementing a robust state machine for tokenization adds complexity but ensures efficient processing of the source code. Balancing simplicity with performance is a challenge.

2. **Flexibility vs. Consistency**: Supporting multiple programming styles and aliases introduces flexibility but can lead to inconsistencies in tokenization. Ensuring consistency across different languages and environments is crucial.

3. **Memory Usage**: Storing the entire source code string and maintaining state variables (position, line, column) requires additional memory. Optimizing memory usage is important for large source files.

4. **Error Reporting**: Providing detailed error messages with accurate line and column information enhances usability but increases the complexity of the lexer implementation.

Overall, the `LexerCore.cpp` file plays a pivotal role in the Quantum Language compiler by converting source code into a structured format of tokens. Through strategic design choices, it balances functionality, efficiency, and maintainability, making it a cornerstone of the compiler's architecture.