# LexerTokenize.cpp

## Role in Compiler Pipeline

`LexerTokenize.cpp` is an essential part of the Quantum Language compiler's pipeline, focusing on the initial lexical analysis phase. During this stage, it transforms the source code into a sequence of tokens, which are then used for subsequent parsing and semantic analysis. The central function, `Lexer::tokenize()`, orchestrates the entire tokenization process.

### Key Design Decisions and Why

The lexer's design prioritizes simplicity and efficiency to ensure quick and accurate processing of the source code. Here are some critical design choices:

- **State Machine Approach**: Utilizing a state machine simplifies the handling of various characters and patterns within the source code, making the implementation more modular and easier to manage.
- **Token Types**: Defining a comprehensive set of token types ensures that all elements of the language are correctly identified and categorized during the lexical analysis phase.
- **Error Handling**: Incorporating robust error handling mechanisms allows the lexer to gracefully report issues such as syntax errors or unrecognized characters, facilitating debugging and maintenance.

### Major Classes/Functions Overview

#### Lexer Class

The `Lexer` class encapsulates the logic for tokenizing the source code. It maintains the current position (`pos`) within the source string, along with tracking the current line and column numbers (`line` and `col`). The class provides methods for reading different types of tokens, skipping whitespace, and reporting errors.

##### Public Methods

- `std::vector<Token> tokenize()`: The main method that initiates the tokenization process and returns a vector of tokens.
- `void skipWhitespace()`: Skips over any whitespace characters in the source code.
- `char current() const`: Returns the character at the current position without advancing the position.
- `char advance()`: Advances the position in the source code and returns the character at the new position.
- `void reportError(const std::string& message)`: Reports an error with the given message at the current position.

#### Token Class

The `Token` class represents a single token in the source code. It includes the type of the token, its lexeme (the actual text of the token), and its location in the source code (line and column).

##### Attributes

- `TokenType type`: Enumerates the type of the token.
- `std::string lexeme`: The textual representation of the token.
- `int line`: The line number where the token starts.
- `int col`: The column number where the token starts.

### Tradeoffs

While the lexer design aims for simplicity and efficiency, there are certain tradeoffs involved:

- **Complexity vs. Simplicity**: A more complex lexer might offer better performance or support more advanced features, but it could also become harder to maintain and debug.
- **Flexibility vs. Robustness**: Allowing greater flexibility in token definitions can make the lexer more adaptable to changes in the language specification, but it might increase the likelihood of errors.
- **Performance vs. Accuracy**: Prioritizing performance might lead to less accurate tokenization, especially for edge cases or unusual constructs in the language.

By carefully balancing these factors, the `LexerTokenize.cpp` file ensures that the initial stages of the Quantum Language compiler are both efficient and reliable.