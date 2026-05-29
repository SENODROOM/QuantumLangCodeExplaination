# LexerCore.cpp Summary

## Role in Compiler Pipeline

The `LexerCore.cpp` file plays a pivotal role in the Quantum Language compiler's pipeline. It is responsible for converting the source code into a series of tokens, serving as the fundamental building blocks for subsequent stages such as parsing and semantic analysis. The lexer meticulously recognizes and categorizes different language elements, ensuring accurate translation and error detection during compilation.

## Key Design Decisions and Why

### Tokenization Strategy
- **Character-Based Parsing**: The lexer processes the source code character-by-character, allowing for precise control over token boundaries and handling special cases like comments and escape sequences.
- **Keyword Recognition**: A comprehensive list of keywords is maintained in an unordered map (`keywords`). This enables efficient lookup and ensures that reserved words are correctly identified and categorized.

### Error Handling
- **Exception Propagation**: Errors encountered during tokenization are thrown using exceptions. This approach provides a clear mechanism for reporting issues directly to the parser or higher-level components.
- **Detailed Error Messages**: Each exception includes detailed information about the error, including the line number and column where it occurred, enhancing debugging capabilities.

### Trade-offs
- **Performance vs. Flexibility**: While the character-based parsing strategy offers high flexibility in handling complex syntax, it may introduce slight performance overhead due to repeated character checks. However, the benefits in terms of accuracy and ease of maintenance outweigh this cost.
- **Memory Usage**: Storing the entire source code string and maintaining additional state variables for position tracking can lead to increased memory usage. To mitigate this, careful optimization techniques are employed to minimize unnecessary data duplication.

## Major Classes/Functions Overview

### Class: Lexer
- **Constructor**: Initializes the lexer with the source code string.
- **Member Variables**:
  - `src`: Stores the source code string.
  - `pos`: Tracks the current position within the source code.
  - `line`: Keeps track of the current line number.
  - `col`: Tracks the current column number.
- **Public Methods**:
  - `tokenize()`: Converts the source code into a vector of tokens.
  - `nextToken()`: Retrieves the next token from the source code.

### Functions
- **current()**: Returns the character at the current position.
- **peek(int offset)**: Returns the character at the specified offset relative to the current position.
- **advance()**: Advances the position pointer and updates line and column numbers accordingly.
- **skipWhitespace()**: Skips all whitespace characters until a non-whitespace character is encountered.
- **skipComment()**: Skips single-line comments starting with `//`.
- **skipBlockComment()**: Skips block comments enclosed within `/* */`.

These functions collectively form the core logic of the lexer, enabling it to accurately parse the source code and generate a stream of tokens that can be used by the parser to construct the AST.