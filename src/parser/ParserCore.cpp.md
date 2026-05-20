# ParserCore.cpp

## Overview

`ParserCore.cpp` is a crucial component of the Quantum Language compiler, responsible for transforming the input source code into an Abstract Syntax Tree (AST). This file contains the implementation of the `Parser` class, which manages both the lexical analysis and syntactic parsing stages during the compilation process.

## Role in Compiler Pipeline

The parser acts as the bridge between the lexer and the rest of the compiler. It takes the sequence of tokens produced by the lexer and constructs a structured AST that represents the program's syntax. The AST serves as the foundation for subsequent semantic analysis and code generation phases.

## Key Design Decisions and Why

1. **Token Stream Management**: The parser maintains a stream of tokens (`std::vector<Token>`), allowing it to sequentially access each token without modifying the original list. This decision ensures that the lexer remains unchanged and facilitates easier debugging and testing.

2. **Lookahead Mechanism**: To handle complex grammatical structures, the parser implements a lookahead mechanism using the `peek()` function. This allows the parser to inspect future tokens without consuming them, enabling more sophisticated parsing rules.

3. **Error Handling**: The parser includes robust error handling through the `expect()` function, which throws exceptions when unexpected token types are encountered. This approach helps in identifying and reporting errors early in the compilation process, improving developer productivity.

4. **Stateful Parsing**: By maintaining a position (`pos`) within the token stream, the parser can backtrack or resume parsing at any point. This statefulness is essential for handling nested structures and recovering from errors gracefully.

5. **Skip Newlines Functionality**: The `skipNewlines()` function simplifies the parsing logic by automatically skipping over newline tokens. This reduces complexity in grammar rules and makes the parser more readable and maintainable.

## Major Classes/Functions Overview

- **Parser Class**:
  - **Constructor**: Initializes the parser with a vector of tokens.
  - **Current Token**: Returns the current token being processed.
  - **Peek Token**: Allows inspection of a token at a specified offset without consuming it.
  - **Consume Token**: Advances the parser to the next token.
  - **Expect Token**: Validates the current token against an expected type, throwing an exception if they do not match.
  - **Check Token**: Checks if the current token matches a specific type.
  - **Match Token**: Consumes the current token if it matches the specified type, returning a boolean indicating success.
  - **At End**: Determines if the parser has reached the end of the token stream.
  - **Skip Newlines**: Automatically skips over consecutive newline tokens.
  - **Parse Function**: Orchestrates the parsing process, constructing an AST by repeatedly calling `parseStatement()` until the end of the token stream is reached.

- **parseStatement() Function**:
  - Parses individual statements from the token stream and returns their corresponding AST nodes.

## Tradeoffs

- **Complexity vs. Readability**: While the lookahead mechanism enhances the parser's ability to handle complex grammars, it also increases the complexity of the codebase. Balancing these factors requires careful consideration of the tradeoff between flexibility and readability.

- **Performance vs. Error Recovery**: Stateful parsing provides powerful capabilities for error recovery but can introduce performance overhead due to backtracking. Optimizing the parser for both speed and robustness is a significant challenge.

- **Memory Usage**: Maintaining a copy of the token stream in memory can be resource-intensive, especially for large programs. Strategies such as lazy evaluation or shared ownership might help mitigate this issue.

In summary, `ParserCore.cpp` is a vital part of the Quantum Language compiler, leveraging advanced parsing techniques to efficiently convert source code into an AST. Its design choices balance flexibility, readability, performance, and memory usage, ensuring a robust and efficient parsing system.