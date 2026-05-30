# ParserCore.cpp

## Overview

`ParserCore.cpp` is a crucial component of the Quantum Language compiler, responsible for converting the input source code into an Abstract Syntax Tree (AST). This file houses the implementation of the `Parser` class, which oversees both the lexical analysis and syntactic parsing phases of the compilation process.

## Role in Compiler Pipeline

The parser acts as the intermediary between the lexer and the rest of the compiler. After the lexer has broken down the source code into individual tokens, the parser takes these tokens and constructs a structured representation of the program's syntax, known as the AST. The AST serves as the foundation for subsequent stages of the compilation process, including semantic analysis, optimization, and code generation.

### Key Design Decisions and Why

1. **Token Stream Management**: The `Parser` class maintains a stream of tokens (`std::vector<Token>`), allowing it to sequentially access each token during parsing. This design choice simplifies the parsing logic by enabling the parser to look ahead at upcoming tokens without modifying the original token stream.

2. **Efficient Token Consumption**: The parser provides methods like `current()`, `peek(int offset)`, and `consume()` to manage the movement through the token stream efficiently. These methods ensure that the parser can easily check the type of the next token or consume the current token, facilitating a clean separation of concerns within the parsing process.

3. **Error Handling**: The `Parser` includes robust error handling mechanisms, such as the `expect` method, which throws a `ParseError` when the expected token type does not match the current token. This ensures that the parser can gracefully handle syntax errors and provide meaningful error messages to the user.

4. **Syntactic Parsing Rules**: The parser defines specific rules for syntactic parsing, such as how statements are constructed and how blocks are nested. These rules are implemented using a series of recursive descent functions (`parseStatement()`, `parseBlock()`, etc.), which make the parsing logic modular and easier to understand.

## Major Classes/Functions Overview

- **Parser Class**:
  - Manages the token stream and provides methods for accessing and consuming tokens.
  - Contains the main parsing function `parse()`, which constructs the AST by recursively parsing statements and blocks.

- **Token Methods**:
  - `current()`: Returns the current token being processed.
  - `peek(int offset)`: Returns the token at a specified offset ahead in the token stream.
  - `consume()`: Consumes the current token and advances to the next one.
  - `expect(TokenType t, const std::string &msg)`: Checks if the current token matches the expected type; otherwise, throws a `ParseError`.
  - `check(TokenType t) const`: Checks if the current token matches the specified type.
  - `match(TokenType t)`: If the current token matches the specified type, consumes it and returns `true`; otherwise, returns `false`.
  - `atEnd() const`: Determines if the end of the token stream has been reached.

- **Utility Functions**:
  - `skipNewlines()`: Skips over any consecutive newline tokens, ensuring that the parser does not treat them as separate statements.

## Tradeoffs

While the parser design offers several advantages, such as simplicity and clarity, there are also some potential tradeoffs:

- **Performance**: The use of recursive descent parsing can lead to performance issues, especially for complex grammars. However, for the relatively straightforward grammar of Quantum Language, this approach remains efficient and easy to implement.

- **Flexibility**: The parser is designed around a fixed set of syntactic rules, which may limit its ability to handle future extensions or variations of the language. To address this, the parser could be refactored to support more flexible grammar definitions, although this would increase complexity.

- **Readability**: The modular design of the parser enhances readability and maintainability, making it easier to add new features or fix bugs. However, the extensive use of recursion might reduce the overall readability for those unfamiliar with the technique.

In summary, `ParserCore.cpp` is a vital part of the Quantum Language compiler, responsible for transforming the input source code into a structured AST. Its design choices and utility functions facilitate efficient and effective parsing, while potential tradeoffs must be considered to balance performance, flexibility, and readability.