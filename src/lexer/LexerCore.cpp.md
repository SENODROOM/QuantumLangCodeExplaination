# LexerCore.cpp Summary

## Role in Compiler Pipeline

The `LexerCore.cpp` file is a crucial component of the Quantum Language compiler's pipeline. It is responsible for converting the source code into a sequence of tokens, which are then processed by the parser to generate an abstract syntax tree (AST). The lexer plays a vital role in the compilation process as it breaks down the input text into meaningful units that can be easily understood and manipulated by subsequent stages of the compiler.

## Key Design Decisions and Why

### Tokenization Rules

The lexer uses predefined rules to identify different types of tokens such as identifiers, literals, operators, and punctuation marks. These rules ensure that the source code is accurately parsed into its constituent parts.

### Handling Whitespace and Comments

- **Whitespace Skipping**: The `skipWhitespace()` function consumes any whitespace characters (' ', '\t', '\r') encountered during tokenization. This helps maintain clean parsing without being affected by formatting issues.
  
- **Single-Line Comment Skipping**: The `skipComment()` function skips over single-line comments starting with `//`. This ensures that these comments do not interfere with the tokenization process.
  
- **Multi-Line Comment Skipping**: The `skipBlockComment()` function handles multi-line comments enclosed within `/* */`. Although the provided code snippet ends abruptly, the implementation would typically consume all characters until it encounters `*/`, effectively skipping over the comment block.

### Error Handling

The lexer includes error handling mechanisms to manage unexpected characters or malformed tokens. For instance, when encountering an unrecognized character, it throws a `std::runtime_error` with a descriptive message indicating the error location and nature.

## Major Classes/Functions Overview

### Lexer Class

The `Lexer` class encapsulates the functionality required for lexical analysis. It maintains the source code string (`src`) along with the current position (`pos`), line number (`line`), and column number (`col`). The constructor initializes these members based on the provided source code.

#### Public Methods

- **Constructor**: Initializes the lexer with the source code.
- **current()**: Returns the character at the current position.
- **peek(offset)**: Returns the character at the specified offset relative to the current position.
- **advance()**: Advances the position to the next character and updates the line and column numbers accordingly.
- **skipWhitespace()**: Skips over all whitespace characters.
- **skipComment()**: Skips over single-line comments.
- **skipBlockComment()**: Skips over multi-line comments.

### Keywords Mapping

A constant unordered map named `keywords` is used to associate specific strings with their corresponding token types. This mapping facilitates quick identification of reserved words and other language-specific tokens.

## Tradeoffs

### Complexity vs. Performance

The use of a comprehensive keyword mapping introduces additional complexity to the lexer but enhances performance by allowing for efficient token recognition. However, this could potentially impact the overall simplicity and readability of the codebase.

### Flexibility vs. Standardization

While the lexer provides flexibility in handling various programming languages and their nuances, it also requires adherence to standard tokenization rules. Balancing between flexibility and strict adherence to standards can be challenging, especially when dealing with multiple target languages.

### Memory Usage vs. Parsing Speed

Storing the entire source code string in memory allows for easy access to individual characters during tokenization. However, this approach may increase memory usage, particularly for large source files. Optimizing memory usage while maintaining fast parsing speeds is a critical tradeoff.

In conclusion, the `LexerCore.cpp` file serves as a fundamental building block for the Quantum Language compiler, providing essential functionality for breaking down source code into tokens. Its design decisions balance accuracy, efficiency, and flexibility, making it a robust and reliable component of the compiler pipeline.