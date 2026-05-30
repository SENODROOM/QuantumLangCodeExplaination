# QuantumLanguage Compiler - Lexer.h

## Overview

The `include/Lexer.h` header file is a crucial component of the QuantumLanguage compiler, responsible for converting source code into tokens. Tokens represent the smallest units of meaningful elements in the language, such as keywords, identifiers, operators, and literals. The Lexer class plays a vital role in the compilation process by breaking down the source code into these tokens, which are then processed by other components like the Parser.

## Role in Compiler Pipeline

The Lexer operates at the beginning of the compiler pipeline, taking the raw source code as input and producing a sequence of tokens. These tokens are fed into the Parser, which constructs the abstract syntax tree (AST). The AST represents the syntactic structure of the program, making it easier for subsequent stages of the compiler to perform semantic analysis, optimization, and code generation.

### Key Design Decisions and Why

1. **State Machine Approach**: The Lexer uses a state machine to efficiently handle different types of characters and transitions between states. This approach simplifies the implementation and improves performance by reducing the need for complex parsing logic.

2. **Support for F-Strings**: To enhance the flexibility and readability of string literals, the Lexer supports f-strings. F-strings allow embedded expressions within string literals, which are expanded at compile time. This feature is implemented using a vector of pending tokens to temporarily store the expanded parts of the f-string.

3. **C Preprocessor Support**: The Lexer includes support for C-style preprocessor directives (`#define`). It maintains a map of macro names to their corresponding token lists, allowing for easy expansion during the lexing phase.

4. **Handling Comments**: The Lexer provides functionality to skip both single-line (`//`) and multi-line (`/* */`) comments. This ensures that comments do not interfere with the tokenization process, maintaining clean and understandable code.

## Major Classes/Functions Overview

### Lexer Class

The `Lexer` class is the primary class defined in this header file. It encapsulates all the necessary functionality for tokenizing the source code.

#### Public Methods

- **explicit Lexer(const std::string &source)**: Constructor that initializes the lexer with the source code.
- **std::vector<Token> tokenize()**: Main method that processes the source code and returns a vector of tokens.

#### Private Methods

- **char current() const**: Returns the current character being processed.
- **char peek(int offset = 1) const**: Returns the character at a specified offset without advancing the position.
- **char advance()**: Advances the position to the next character and returns it.
- **void skipWhitespace()**: Skips any whitespace characters in the source code.
- **void skipComment()**: Skips a single-line comment starting with `//`.
- **void skipBlockComment()**: Skips a multi-line comment enclosed by `/* */`.
- **Token readNumber()**: Reads a numeric literal from the source code and returns it as a `Token`.
- **Token readString(char quote)**: Reads a string literal from the source code, handling both single and double quotes, and returns it as a `Token`.
- **void readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)**: Handles the expansion of f-strings and appends the resulting tokens to the output vector.
- **Token readIdentifierOrKeyword()**: Reads an identifier or keyword from the source code and returns it as a `Token`.
- **Token readOperator()**: Reads an operator from the source code and returns it as a `Token`.

### Keywords Map

The `keywords` static unordered map stores common keywords of the QuantumLanguage and associates them with their respective `TokenType`. This allows for quick lookup and classification of identifiers during the lexing process.

### Tradeoffs

1. **Performance vs. Flexibility**: While the state machine approach enhances performance, supporting features like f-strings and C preprocessor directives adds complexity and potential overhead. Balancing these features against performance requirements was a key design decision.

2. **Readability vs. Complexity**: Supporting f-strings and C preprocessor directives increases the complexity of the Lexer class. However, these features improve the readability and usability of the source code, making the overall development experience more pleasant.

3. **Memory Usage**: Storing macro definitions in memory can lead to increased memory usage, especially for large projects with many macros. Careful management of memory allocation and deallocation was necessary to minimize this impact.

By understanding the role of the Lexer in the QuantumLanguage compiler pipeline and its key design decisions, developers can better appreciate how this component contributes to the overall efficiency and effectiveness of the compiler.