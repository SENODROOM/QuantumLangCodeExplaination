# LexerTokenize.cpp

## Role in Compiler Pipeline

`LexerTokenize.cpp` is a crucial component of the Quantum Language compiler, responsible for the initial step of lexical analysis. This stage converts the source code into a series of tokens that serve as the foundation for further parsing and semantic analysis. The primary function, `Lexer::tokenize()`, manages the overall tokenization process.

## Key Design Decisions and Why

The lexer is designed to handle various types of characters and sequences within the source code, including identifiers, numbers, strings, and special symbols like comments and preprocessor directives. Here are some key design decisions:

- **Character Classification**: Utilizes character classification functions (`std::isspace`, `std::isdigit`, etc.) to identify different types of characters efficiently.
- **State Machine Approach**: Implements a state machine to manage transitions between different states during tokenization. This approach ensures that complex patterns can be accurately recognized without significant overhead.
- **Preprocessor Handling**: Specifically handles preprocessor directives such as `#define`. It reads these directives and processes their arguments, expanding macros where necessary.
- **Error Reporting**: Integrates error reporting mechanisms to handle syntax errors gracefully, providing clear feedback on issues encountered during tokenization.

These decisions were made to ensure robustness, efficiency, and ease of maintenance in the lexer implementation.

## Major Classes/Functions Overview

### Lexer Class

The `Lexer` class encapsulates the logic for reading and processing the source code. It contains member variables to track the current position (`pos`), line number (`line`), and column number (`col`). The class provides methods for advancing the position, skipping whitespace, and identifying different token types.

#### Methods

- `void Lexer::advance()`: Advances the current position in the source code by one character.
- `char Lexer::current() const`: Returns the character at the current position.
- `void Lexer::skipWhitespace()`: Skips over any whitespace characters at the current position.
- `std::vector<Token> Lexer::tokenize()`: Orchestrates the tokenization process, returning a vector of `Token` objects.

### Token Class

The `Token` class represents an individual token produced by the lexer. Each token has a type (`TokenType`), a lexeme (the actual text of the token), and positional information (`startLine`, `startCol`) to help with error reporting and debugging.

### Error Class

The `Error` class is used to report syntax errors encountered during tokenization. It provides methods for creating and displaying error messages, ensuring that the compiler can provide useful feedback to the user.

## Tradeoffs

While the lexer is designed to handle a wide range of cases, there are some inherent tradeoffs:

- **Complexity vs. Performance**: The use of a state machine adds complexity but improves performance by allowing efficient recognition of token patterns.
- **Flexibility vs. Simplicity**: Supporting advanced features like macro expansion requires additional complexity compared to a simpler lexer.
- **Memory Usage**: Storing intermediate tokens and handling large source files can lead to increased memory usage.

Despite these tradeoffs, the lexer remains a vital and efficient part of the Quantum Language compiler, enabling accurate and reliable parsing of the source code.