# LexerTokenize.cpp

## Role in Compiler Pipeline

`LexerTokenize.cpp` is a crucial component of the Quantum Language compiler's lexer phase. This file contains the implementation of the `Lexer::tokenize()` method, which processes the source code into a sequence of tokens. These tokens serve as the fundamental building blocks for subsequent phases of the compiler, including parsing, semantic analysis, and code generation.

## Key Design Decisions and Why

### Tokenization Strategy
The primary strategy employed in `LexerTokenize.cpp` is to iterate through the source code character-by-character, identifying and categorizing each character or sequence of characters into specific token types. This approach ensures that every part of the input is processed accurately and efficiently.

### Handling Whitespace and Comments
- **Whitespace Skipping**: The `skipWhitespace()` function is used to ignore unnecessary spaces and tabs, ensuring that only meaningful characters contribute to the token stream.
- **Comments**: Single-line comments starting with `#` are handled by skipping them entirely during the tokenization process. This decision simplifies the parser's job by removing commented-out text before it begins processing.

### Macro Expansion
Macros are identified and expanded within the tokenizer. When a `#define` directive is encountered, the macro name and its replacement value are read. The replacement value is then parsed as a series of tokens, allowing macros to be used seamlessly throughout the rest of the compilation process.

## Major Classes/Functions Overview

### Lexer Class
- **Purpose**: Manages the state and operations related to the lexical analysis of the source code.
- **Methods**:
  - `tokenize()`: Main method that processes the source code and returns a vector of tokens.
  - `skipWhitespace()`: Skips over any whitespace characters in the source code.
  - `readNumber()`: Reads and returns a numeric literal token from the source code.
  - `readString(char delimiter)`: Reads and returns a string literal token from the source code using the specified delimiter.

### Token Class
- **Purpose**: Represents a single token produced by the lexer.
- **Attributes**:
  - `type`: The type of the token (e.g., IDENTIFIER, NUMBER, STRING).
  - `value`: The textual representation of the token.
  - `line`: The line number where the token was found.
  - `col`: The column number where the token was found.

### Error Class
- **Purpose**: Handles error reporting during the lexical analysis phase.
- **Methods**:
  - `reportError(std::string message, int line, int col)`: Reports an error at the specified line and column with the given message.

## Tradeoffs

### Performance vs. Simplicity
While the current implementation provides a straightforward way to tokenize the source code, it might not be the most efficient for very large inputs. Future optimizations could involve more complex data structures or algorithms to improve performance without sacrificing simplicity.

### Flexibility vs. Complexity
The ability to handle macros adds flexibility but increases complexity in the lexer. Managing macro expansions requires careful consideration of how identifiers should be interpreted and resolved, potentially complicating the overall design of the compiler.

### Error Handling
The current error handling mechanism is basic and does not provide detailed information about the context in which errors occur. Improving error messages could help developers understand and fix issues more effectively.

In summary, `LexerTokenize.cpp` plays a vital role in the Quantum Language compiler by breaking down the source code into manageable tokens. Its design choices balance functionality with maintainability, although there are areas for potential improvement in terms of performance, flexibility, and error handling.