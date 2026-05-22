# LexerTokenize.cpp

## Role in Compiler Pipeline

`LexerTokenize.cpp` is a crucial component of the Quantum Language compiler, responsible for the lexical analysis phase. This stage converts the source code into a sequence of tokens, forming the basis for further parsing and semantic analysis. The primary function, `Lexer::tokenize()`, manages the tokenization process, ensuring that each character in the source code is correctly identified and categorized into appropriate token types.

## Key Design Decisions and Why

### Token Types Handling

The lexer defines various token types such as keywords, identifiers, numbers, strings, operators, and special characters. Each token type has its unique representation to facilitate easy parsing and semantic analysis.

**Why:** Differentiating between these types allows the parser to construct the abstract syntax tree accurately, making it easier to identify errors and perform optimizations during compilation.

### Error Handling

The lexer includes error handling mechanisms to manage unexpected characters or malformed tokens gracefully. Errors are reported using custom error messages, providing clear feedback on issues encountered during the lexical analysis phase.

**Why:** Robust error handling ensures that the compiler can provide meaningful diagnostics to developers, helping them quickly locate and fix problems in their source code.

### Macro Expansion

The lexer supports basic macro expansion, allowing the preprocessor to handle simple macros defined in the source code. Macros are expanded inline, reducing the complexity of later stages of the compiler.

**Why:** Macro expansion simplifies the source code by replacing repetitive patterns with more concise alternatives, improving maintainability and readability.

## Major Classes/Functions Overview

### Lexer Class

- **Purpose:** Manages the lexical analysis process, converting source code into tokens.
- **Key Functions:**
  - `void skipWhitespace():` Skips over any whitespace characters in the source code.
  - `char current():` Returns the current character being processed.
  - `int readNumber():` Reads a numeric token from the source code.
  - `Token readString(char delimiter):` Reads a string token from the source code, handling both single and double quotes.
  - `Token readIdentifier():` Reads an identifier token from the source code, checking for macro definitions.
  - `std::vector<Token> tokenize():` Orchestrates the entire tokenization process, returning a vector of tokens.

### Token Struct

- **Purpose:** Represents a single token produced by the lexer.
- **Fields:**
  - `TokenType type:` The type of the token (e.g., IDENTIFIER, NUMBER, STRING).
  - `std::string text:` The textual representation of the token.
  - `int line:` The line number where the token starts.
  - `int column:` The column number where the token starts.

## Tradeoffs

### Complexity vs. Flexibility

While supporting multiple token types and macro expansion adds flexibility, it also increases the complexity of the lexer implementation. Balancing these features requires careful consideration of performance and maintainability.

### Error Reporting Precision

Enhanced error reporting improves developer experience but may increase the lexer's overhead. Finding the precise location and nature of errors requires sophisticated parsing techniques, which can complicate the implementation.

### Macro Expansion Limitations

Basic macro expansion is limited to one level of substitution. Supporting deeper macro expansions would require additional complexity and potentially impact performance. Carefully evaluating the use cases and limitations helps in deciding whether to implement more advanced macro handling.

In conclusion, `LexerTokenize.cpp` is a vital part of the Quantum Language compiler, enabling efficient lexical analysis and setting the stage for more complex parsing and semantic analysis tasks. Its design decisions and functionality ensure robustness, precision, and scalability, contributing significantly to the overall quality and reliability of the compiler.