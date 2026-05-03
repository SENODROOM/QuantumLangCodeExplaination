# LexerTokenize.cpp

## Role in Compiler Pipeline

`LexerTokenize.cpp` plays a pivotal role in the Quantum Language compiler's lexical analysis phase. It implements the `Lexer::tokenize()` method, which converts the source code into a stream of tokens. These tokens are essential for the parser to construct an abstract syntax tree accurately.

## Key Design Decisions and Why

1. **State Machine Approach**: The lexer uses a state machine to efficiently parse the input text. This approach minimizes complexity and ensures that all possible token sequences can be handled correctly.

2. **Whitespace Skipping**: Whitespace characters like spaces and tabs are automatically skipped during tokenization. This simplifies the parsing process by removing unnecessary clutter.

3. **Directive Handling**: Special directives starting with `#` are recognized and processed separately. For example, the `#define` directive is used to define macros, which are then expanded during preprocessing.

4. **Macro Expansion**: Macros are expanded inline within the source code. This allows for more flexible and readable code, but requires careful handling to avoid infinite recursion or incorrect expansions.

5. **Error Reporting**: Errors encountered during tokenization are reported using the `Error` class. This provides clear feedback to the user about issues in their source code.

## Major Classes/Functions Overview

### Lexer Class

- **Constructor**: Initializes the lexer with the source code string.
- **tokenize() Method**: Main function that processes the source code and returns a vector of tokens.
- **skipWhitespace() Function**: Skips over any whitespace characters in the input.
- **advance() Function**: Advances the position pointer in the source code.
- **current() Function**: Returns the character at the current position in the source code.

### Token Class

- Represents a single token in the source code.
- Contains properties such as type, lexeme, line number, and column number.

### readNumber() Function

- Parses a numeric literal from the source code and returns it as a `Token`.

### readString(char delimiter) Function

- Parses a string literal from the source code using the specified delimiter (`'` or `"`) and returns it as a `Token`.

### Error Class

- Handles error reporting in the lexer.
- Provides methods to report errors with specific messages and locations.

## Tradeoffs

1. **Complexity vs. Simplicity**: Using a state machine adds some complexity to the lexer implementation, but significantly simplifies the overall parsing process by reducing the need for nested loops and conditionals.

2. **Performance**: The state machine approach generally offers better performance compared to recursive descent parsers, especially for larger source codes.

3. **Flexibility vs. Correctness**: Allowing macros introduces flexibility in the language, but also increases the potential for errors if not handled carefully. Ensuring correct macro expansion without causing infinite recursion is a challenging tradeoff.

4. **Readability vs. Efficiency**: While skipping whitespace and handling directives makes the lexer easier to understand, it might slightly impact performance. However, the benefits in terms of code readability and maintainability often outweigh these costs.

Overall, `LexerTokenize.cpp` is a well-designed and efficient component of the Quantum Language compiler, balancing simplicity, correctness, and flexibility in its implementation.