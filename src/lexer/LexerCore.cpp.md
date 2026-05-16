# LexerCore.cpp Summary

## Role in Compiler Pipeline

The `LexerCore.cpp` file plays a critical role in the Quantum Language compiler's pipeline. It converts the source code into a sequence of tokens, forming the basis for subsequent parsing into an Abstract Syntax Tree (AST). This process involves recognizing and categorizing various elements of the language, such as keywords, identifiers, literals, operators, and punctuation marks, ensuring accurate translation for the parser.

## Key Design Decisions and Why

### Tokenization Rules

- **Character Classification**: The file uses character classification functions (`std::isspace`, `std::isdigit`, etc.) to identify different types of characters in the source code.
- **Keyword Mapping**: A predefined map (`keywords`) associates specific strings with their corresponding token types, enabling efficient keyword recognition.
- **State Machine Approach**: The lexer employs a state machine to handle complex scenarios like multi-character operators, string literals, and comments, ensuring robustness and flexibility.

### Error Handling

- **Exception Propagation**: Errors encountered during tokenization are propagated using exceptions, providing clear insights into issues within the source code.
- **Custom Error Messages**: The file includes custom error messages tailored to specific lexical errors, enhancing readability and maintainability.

## Major Classes/Functions Overview

### Lexer Class

- **Constructor**: Initializes the lexer with the source code and sets up initial positions and line/column tracking.
- **Public Methods**:
  - `tokenize()`: Main method to tokenize the input source code.
  - `nextToken()`: Retrieves the next token from the source code.
  - `error(std::string msg)`: Throws a custom exception with the provided message, indicating a lexical error.

### Private Helper Functions

- `current()`: Returns the current character being processed.
- `peek(int offset)`: Looks ahead at the specified number of characters without advancing the position.
- `advance()`: Advances the position to the next character and updates line/column information.
- `skipWhitespace()`: Skips over any whitespace characters in the source code.
- `skipComment()`: Skips over single-line comments starting with `//`.
- `skipBlockComment()`: Skips over block comments enclosed between `/*` and `*/`.

## Tradeoffs

- **Performance vs. Complexity**: While the use of a state machine allows for handling complex cases efficiently, it also increases the complexity of the implementation. Balancing these aspects is crucial for maintaining both performance and maintainability.
- **Flexibility vs. Standardization**: Providing aliases for certain keywords (e.g., `self` for `this` in Quantum) offers flexibility but may complicate standardization efforts. Careful consideration must be given to ensure consistency across different programming languages.
- **Readability vs. Efficiency**: Custom error messages improve readability but might slightly impact efficiency. Finding an optimal balance is essential for overall usability.

This comprehensive approach to tokenization ensures that the Quantum Language compiler can accurately parse and understand its source code, laying the groundwork for more sophisticated compilation processes.