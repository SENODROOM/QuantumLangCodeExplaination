# LexerTokenize.cpp

## Role in Compiler Pipeline

`LexerTokenize.cpp` is a fundamental part of the Quantum Language compiler's pipeline, dedicated to the initial phase of lexical analysis. This step transforms the source code into a sequence of tokens, which are essential for subsequent parsing and semantic analysis stages. The central function, `Lexer::tokenize()`, orchestrates the entire tokenization process.

## Key Design Decisions and Why

The lexer is designed with several key considerations:

1. **Efficiency**: To handle large source files efficiently, the lexer uses a single pass through the input string, minimizing overhead.
2. **Flexibility**: It supports various types of tokens including identifiers, numbers, strings, and directives, ensuring broad language coverage.
3. **Error Handling**: Robust error handling mechanisms are implemented to manage unexpected characters or syntax errors gracefully.
4. **Macro Expansion**: The lexer includes basic support for macro definition and expansion, enhancing its capability to preprocess the source code.

These decisions were made to ensure that the lexer can effectively parse the Quantum Language source code, providing a solid foundation for the compiler's further operations.

## Major Classes/Functions Overview

### Lexer Class

- **Purpose**: Manages the state and logic for tokenizing the source code.
- **Key Functions**:
  - `tokenize()`: Orchestrates the tokenization process.
  - `skipWhitespace()`: Skips over any leading whitespace characters.
  - `readNumber()`: Reads a numeric literal from the source code.
  - `readString(char delimiter)`: Reads a string literal from the source code using the specified delimiter.
  - `advance()`: Advances the position pointer to the next character in the source code.

### Token Class

- **Purpose**: Represents a single token extracted from the source code.
- **Attributes**:
  - `type`: The type of the token (e.g., IDENTIFIER, NUMBER, STRING).
  - `value`: The string representation of the token.
  - `line`: The line number where the token starts.
  - `col`: The column number where the token starts.

### TokenType Enum

- **Purpose**: Defines the different types of tokens that can be produced during lexical analysis.
- **Values**:
  - `IDENTIFIER`
  - `NUMBER`
  - `STRING`
  - `NEWLINE`
  - `DIRECTIVE`

## Tradeoffs

While the lexer provides comprehensive support for tokenizing the Quantum Language source code, it also comes with certain tradeoffs:

1. **Complexity**: The inclusion of macro expansion adds complexity to the lexer, requiring additional logic to handle these cases.
2. **Performance**: Although efficient, the lexer still needs to perform multiple checks and transitions between states, which could potentially impact performance on very large inputs.
3. **Maintainability**: Supporting both simple and complex tokens might make the lexer harder to maintain and extend in the future.

Despite these tradeoffs, the lexer remains a critical and well-designed component of the Quantum Language compiler, enabling accurate and efficient lexical analysis.