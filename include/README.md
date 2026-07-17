# QuantumLanguage Compiler - Lexer.h

## Overview

The `include/Lexer.h` header file is an essential component of the QuantumLanguage compiler, focusing on the lexical analysis phase. This phase involves breaking down the source code into individual tokens, which are then processed by subsequent phases of the compiler. The Lexer class is central to this process, ensuring accurate and efficient tokenization.

## Role in Compiler Pipeline

The Lexer operates at the beginning of the compiler pipeline, taking raw source code as input and producing a stream of tokens. These tokens represent the smallest units of meaning in the language, such as keywords, identifiers, operators, and literals. The output of the Lexer is consumed by the Parser, which constructs a syntax tree from these tokens.

## Key Design Decisions and Why

1. **Token Types**: The Lexer uses an enumeration (`TokenType`) to categorize different types of tokens. This allows for clear identification and processing of each type during compilation.
   
2. **State Management**: The Lexer maintains state variables like `pos`, `line`, and `col` to track its position within the source code. This helps in generating accurate error messages and preserving the context of each token.

3. **Handling Comments**: The Lexer includes methods to skip both single-line (`// ...`) and block comments (`/* ... */`). This ensures that comments do not interfere with the tokenization process and are effectively ignored.

4. **F-String Expansion**: The Lexer supports f-string expansion, storing pending tokens in `pendingTokens_`. This feature enhances string manipulation capabilities within the language.

5. **C Preprocessor Macros**: The Lexer can handle C-style preprocessor directives (`#define`). It stores macro definitions in `defines_`, allowing for macro expansion during the compilation process.

## Major Classes/Functions Overview

### Lexer Class

- **Constructor**: `explicit Lexer(const std::string &source)` initializes the Lexer with the source code.
  
- **tokenize Function**: `std::vector<Token> tokenize()` processes the source code and returns a vector of tokens.

### Private Member Functions

- **current Function**: Returns the current character being analyzed.
  
- **peek Function**: Returns the character at the specified offset without advancing the lexer's position.
  
- **advance Function**: Advances the lexer's position and returns the character that was previously at the current position.
  
- **skipWhitespace Function**: Skips over any whitespace characters in the source code.
  
- **skipComment Function**: Skips over a single-line comment starting with `//`.
  
- **skipBlockComment Function**: Skips over a multi-line comment enclosed between `/*` and `*/`.
  
- **readNumber Function**: Reads a numeric literal from the source code and returns it as a `Token`.
  
- **readString Function**: Reads a string literal from the source code, handling both single and double quotes. Returns the string as a `Token`.
  
- **readTemplateLiteral Function**: Handles template literals, expanding them into a sequence of tokens. Used for f-string support.
  
- **readIdentifierOrKeyword Function**: Reads an identifier or keyword from the source code and returns it as a `Token`.
  
- **readOperator Function**: Reads an operator from the source code and returns it as a `Token`.

## Tradeoffs

- **Complexity vs. Performance**: Supporting features like f-string expansion and C preprocessor macros adds complexity to the Lexer. However, these features enhance the language's usability and flexibility, potentially leading to better performance through more efficient code generation.

- **Error Handling**: The Lexer must be robust in handling errors, such as unterminated strings or invalid escape sequences. While this increases complexity, it ensures that the compiler provides meaningful error messages to help developers debug their code.

- **Maintainability vs. Extensibility**: Adding new token types or handling additional language features requires modifications to the Lexer. Balancing maintainability and extensibility is crucial for long-term project success.

Overall, the `Lexer.h` file is a vital part of the QuantumLanguage compiler, providing the foundation for accurate and efficient tokenization. Its design decisions reflect a balance between language features, performance, and error handling, making it a well-engineered component of the compiler architecture.