# ParserCore.cpp

## Overview

`ParserCore.cpp` is a critical component of the Quantum Language compiler, responsible for converting the input source code into an Abstract Syntax Tree (AST). This file houses the implementation of the `Parser` class, which oversees both the lexical analysis and syntactic parsing phases of the compilation process.

## Role in Compiler Pipeline

The parser acts as the bridge between the lexer and the rest of the compiler. After the lexer has tokenized the source code, the parser takes these tokens and constructs a structured AST that represents the program's syntax. This AST is then used by subsequent stages of the compiler to perform semantic analysis, optimization, and code generation.

### Key Design Decisions and Why

1. **Token Stream Management**: The parser maintains a stream of tokens and provides methods like `current`, `peek`, `consume`, `expect`, `check`, and `match` to navigate through this stream efficiently. These methods ensure that the parser can look ahead at upcoming tokens without consuming them prematurely, which is essential for handling complex grammatical structures.

2. **Error Handling**: The `expect` method throws a `ParseError` when it encounters an unexpected token, providing context such as the expected token type and the actual token received. This helps in diagnosing issues early in the compilation process.

3. **Syntactic Parsing**: The parser uses recursive descent parsing, a top-down parsing technique, to build the AST. Each function in the parser corresponds to a non-terminal in the grammar, making the code easy to understand and maintain.

4. **Skip Newlines**: The `skipNewlines` method allows the parser to ignore multiple consecutive newline tokens, simplifying the handling of whitespace in the source code.

## Major Classes/Functions Overview

- **Parser Class**:
  - **Constructor**: Initializes the parser with a vector of tokens.
  - **Methods**:
    - `current()`: Returns the current token being processed.
    - `peek(int offset)`: Returns the token at the specified offset ahead of the current position.
    - `consume()`: Consumes the current token and advances to the next one.
    - `expect(TokenType t, const std::string &msg)`: Ensures the current token matches the expected type, throwing an error otherwise.
    - `check(TokenType t) const`: Checks if the current token matches the given type.
    - `match(TokenType t)`: If the current token matches the given type, consumes it and returns true; otherwise, returns false.
    - `atEnd() const`: Determines if the parser has reached the end of the token stream.
    - `skipNewlines()`: Advances past any consecutive newline tokens.
    - `parse()`: Parses the entire token stream into an AST.

- **ASTNode Class**:
  - Represents nodes in the Abstract Syntax Tree.
  - Contains information about the node's type, value, and children.

- **BlockStmt Class**:
  - Represents a block of statements in the quantum language.
  - Used to construct the AST for compound statements.

## Tradeoffs

- **Flexibility vs. Complexity**: Recursive descent parsing offers high flexibility but can lead to complex and hard-to-maintain code, especially for large grammars. However, this approach is straightforward and intuitive for implementing simple parsers.

- **Performance**: Token stream management and error handling add overhead to the parser. Efficient data structures and algorithms are used to mitigate these impacts, ensuring good performance even for larger programs.

- **Readability vs. Conciseness**: The use of descriptive method names and clear structure enhances readability, making the code easier to understand and debug. However, this might come at the cost of some conciseness in certain parts of the implementation.

Overall, `ParserCore.cpp` plays a vital role in transforming the raw tokens produced by the lexer into a structured AST, enabling further processing by the compiler. Its design choices balance flexibility, performance, and readability to create a robust and maintainable parsing system.