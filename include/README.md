# QuantumLanguage Compiler - Lexer.h

## Overview

The `include/Lexer.h` header file is an essential part of the QuantumLanguage compiler, focusing on the lexical analysis phase. It is responsible for breaking down the source code into meaningful tokens that can be processed by subsequent phases of the compiler. The lexer plays a critical role in converting high-level programming concepts into low-level instructions that the compiler understands.

### Key Design Decisions and Why

- **Efficient Tokenization**: The lexer uses a combination of character reading and state transitions to efficiently parse the input source code. This approach minimizes overhead and ensures fast processing.
  
- **Support for F-Strings**: To enhance readability and support dynamic string formatting, the lexer includes mechanisms for handling f-strings. These are strings that allow embedded expressions, which are evaluated at runtime.

- **C Preprocessor Compatibility**: The lexer supports C-style preprocessor directives (`#define`) by maintaining a map of macro names to their replacement token lists. This allows the compiler to preprocess the source code before further compilation steps.

- **Error Handling**: While not explicitly shown in the provided code snippet, the lexer would typically include error handling mechanisms to manage syntax errors gracefully, providing useful feedback to the user.

## Major Classes/Functions Overview

### Lexer Class

The `Lexer` class is central to the lexical analysis process. It takes a source code string as input and produces a vector of `Token` objects representing the parsed elements of the code.

#### Public Methods

- **Constructor (`explicit Lexer(const std::string &source)`)**: Initializes the lexer with the given source code.
- **tokenize()**: Main method that processes the source code and returns a vector of tokens.

#### Private Methods

- **current() const**: Returns the current character being analyzed.
- **peek(int offset = 1) const**: Returns the character at the specified offset ahead without advancing the position.
- **advance()**: Advances the lexer's position to the next character and returns it.
- **skipWhitespace()**: Skips over any whitespace characters in the source code.
- **skipComment()**: Skips over single-line comments starting with `//`.
- **skipBlockComment()**: Skips over multi-line comments enclosed between `/*` and `*/`.
- **readNumber()**: Parses numeric literals and returns them as tokens.
- **readString(char quote)**: Parses string literals enclosed by the specified quote character and returns them as tokens.
- **readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)**: Parses template literals (f-strings) and stores the resulting tokens in the output vector.
- **readIdentifierOrKeyword()**: Parses identifiers or reserved keywords and returns them as tokens.
- **readOperator()**: Parses operators and punctuation marks and returns them as tokens.

### Token Class

The `Token` class represents individual lexical elements found during the parsing process. Each token has properties such as its type, value, and location in the source code.

## Tradeoffs

- **Complexity vs. Performance**: The lexer's design balances simplicity with performance. By using direct character manipulation and state transitions, the lexer achieves efficient parsing but may become more complex as new features like f-strings and preprocessor directives are added.
  
- **Flexibility vs. Simplicity**: Supporting advanced features like f-strings and C preprocessor directives increases the lexer's flexibility but also adds complexity and potential for bugs. Simplifying these features could reduce complexity but might limit the language's capabilities.

- **Readability vs. Efficiency**: The use of C-style preprocessor directives and f-strings enhances the readability of the source code but requires additional parsing logic in the lexer. Balancing these factors ensures that the lexer remains both readable and efficient.