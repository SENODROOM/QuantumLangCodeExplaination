# QuantumLanguage Compiler - Lexer.h

## Overview

The `Lexer.h` header file is an essential part of the QuantumLanguage compiler, responsible for converting source code into a sequence of tokens. The lexer is the first stage in the compilation process, taking raw text as input and producing a stream of tokens that represent the syntactic structure of the language. These tokens are then consumed by the parser to construct an abstract syntax tree (AST), which serves as the foundation for further stages of compilation such as semantic analysis and code generation.

### Key Design Decisions and Why

1. **Tokenization**: The primary responsibility of the lexer is to break down the source code into individual tokens. Each token represents a meaningful unit of the language, such as keywords, identifiers, operators, and literals. By separating the source code into tokens, the lexer facilitates the parsing process, making it easier to understand the structure of the program.

2. **Error Handling**: The lexer includes mechanisms for error detection and reporting. If it encounters characters or sequences that do not form valid tokens, it generates appropriate error messages. This helps developers identify and fix issues in their source code early in the development process.

3. **State Machine**: To efficiently handle different types of tokens, the lexer employs a state machine approach. This allows it to transition between various states based on the characters encountered in the source code. For example, when reading a number, the lexer can switch to a state where it continues to read digits until it encounters a non-digit character.

4. **Support for Templates**: The lexer includes support for template literals, which are a feature of modern programming languages. Template literals allow for string interpolation within strings, making them more flexible and powerful. The lexer handles these literals by expanding them into a sequence of tokens, which can then be processed by subsequent stages of the compiler.

5. **Preprocessor Macros**: The lexer also supports C-style preprocessor macros. Macros allow developers to define reusable code snippets that can be expanded during the preprocessing phase. The lexer maintains a map of macro names to their corresponding replacement token lists, enabling it to expand macros as they are encountered in the source code.

### Major Classes/Functions Overview

- **Lexer Class**:
  - **Constructor (`explicit Lexer(const std::string &source)`)**: Initializes the lexer with the source code to be tokenized.
  - **tokenize Function (`std::vector<Token> tokenize()`)**: Converts the source code into a vector of tokens.

- **Private Member Functions**:
  - **current Function (`char current() const`)**: Returns the current character being processed.
  - **peek Function (`char peek(int offset = 1) const`)**: Returns the character at the specified offset without advancing the position.
  - **advance Function (`char advance()`)**: Advances the position to the next character and returns it.
  - **skipWhitespace Function ()**: Skips over any whitespace characters in the source code.
  - **skipComment Function ()**: Skips over single-line comments starting with `//`.
  - **skipBlockComment Function ()**: Skips over multi-line comments enclosed within `/* */`.
  - **readNumber Function (`Token readNumber()`)**: Reads a numeric literal from the source code and returns it as a token.
  - **readString Function (`Token readString(char quote)`)**: Reads a string literal from the source code using the specified quote character and returns it as a token.
  - **readTemplateLiteral Function (`void readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)`)**: Expands a template literal into a sequence of tokens and stores them in the provided output vector.
  - **readIdentifierOrKeyword Function (`Token readIdentifierOrKeyword()`)**: Reads an identifier or keyword from the source code and returns it as a token.
  - **readOperator Function (`Token readOperator()`)**: Reads an operator or punctuation mark from the source code and returns it as a token.

### Tradeoffs

- **Performance vs. Flexibility**: While the lexer's use of a state machine provides flexibility in handling different types of tokens, it may introduce some performance overhead compared to simpler approaches. However, the benefits of robust tokenization and error handling outweigh the potential performance impact.

- **Complexity vs. Simplicity**: Supporting features like template literals and preprocessor macros adds complexity to the lexer implementation. However, these features enhance the expressiveness and usability of the language, making the overall system more valuable.

- **Memory Usage**: Storing the source code and intermediate data structures requires additional memory. While this can be a concern, the benefits of having a clear separation of concerns and easy-to-manage data flow typically justify the increased memory usage.

In conclusion, the `Lexer.h` header file is a critical component of the QuantumLanguage compiler, responsible for breaking down the source code into meaningful tokens. Its design decisions and functionality ensure efficient and accurate tokenization, while its tradeoffs provide a balance between performance, flexibility, and complexity.