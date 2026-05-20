# LexerCore.cpp Summary

## Role in Compiler Pipeline

The `LexerCore.cpp` file is a crucial component of the Quantum Language compiler's pipeline. Its primary function is to convert the source code into a sequence of tokens, which serve as the foundation for further processing into an Abstract Syntax Tree (AST). The lexer handles the recognition and categorization of various language elements, including keywords, identifiers, literals, operators, and punctuation marks.

## Key Design Decisions and WHY

1. **Token Types**: The lexer uses a predefined set of token types (`TokenType`) to classify different parts of the source code. These types include keywords, identifiers, literals, operators, and punctuation marks. By defining these types explicitly, the lexer ensures that each element is correctly categorized and processed during the compilation phase.

2. **State Machine Approach**: To efficiently recognize complex patterns like strings or numbers, the lexer employs a state machine approach. Each character in the source code transitions the lexer between different states based on its context. For example, when encountering a double quote, the lexer enters a string state until another double quote is found. This method allows for precise control over the tokenization process and minimizes the complexity of handling edge cases.

3. **Error Handling**: The lexer includes robust error handling mechanisms to manage unexpected characters or malformed input gracefully. When an error is detected, it throws exceptions with descriptive error messages, facilitating easier debugging and maintenance of the compiler.

4. **Line and Column Tracking**: To provide accurate error reporting and support features like syntax highlighting, the lexer tracks the current line and column number in the source code. This information is essential for pinpointing errors and enhancing user experience.

## Major Classes/Functions Overview

### Lexer Class

- **Constructor**: Initializes the lexer with the source code and sets up initial tracking variables (`src`, `pos`, `line`, `col`).
- **current()**: Returns the current character being processed.
- **peek(offset)**: Returns the character at the specified offset ahead without advancing the position.
- **advance()**: Advances the position to the next character and updates line and column tracking accordingly.
- **skipWhitespace()**: Skips all whitespace characters (' ', '\t', '\r') until a non-whitespace character is encountered.
- **skipComment()**: Skips single-line comments starting with '#' until the end of the line.
- **skipBlockComment()**: Skips block comments enclosed within '/*' and '*/'. Note: The implementation provided only shows the start of the function, indicating that the full logic for skipping block comments is missing.

### TokenType Enum

- Defines an enumeration of possible token types used by the lexer. Common types include keywords, identifiers, literals, operators, and punctuation marks.

## Tradeoffs

1. **Complexity vs. Performance**: While using a state machine approach provides flexibility and accuracy in tokenization, it can introduce additional complexity and overhead compared to simpler methods. Balancing these factors requires careful consideration of the specific needs and constraints of the Quantum Language compiler.

2. **Flexibility vs. Maintainability**: Providing extensive support for various programming paradigms and language features increases the flexibility of the lexer but may also complicate its maintainability. Ensuring that the lexer remains modular and easy to extend is crucial for long-term success of the compiler.

3. **Memory Usage vs. Tokenization Accuracy**: Storing a large map of keywords and their corresponding token types can increase memory usage. However, maintaining this map ensures that the lexer accurately recognizes and categorizes keywords, which is essential for correct compilation.

In conclusion, the `LexerCore.cpp` file is a vital part of the Quantum Language compiler's pipeline, responsible for converting source code into a stream of tokens. Through strategic design choices and comprehensive error handling, the lexer ensures efficient and accurate tokenization, setting the stage for successful parsing and AST generation.