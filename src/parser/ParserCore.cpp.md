# ParserCore.cpp

## Overview

`ParserCore.cpp` is a critical component of the Quantum Language compiler, responsible for converting the input source code into an Abstract Syntax Tree (AST). This file houses the implementation of the `Parser` class, orchestrating both the lexical analysis and syntactic parsing phases of the compilation process.

## Role in Compiler Pipeline

The parser plays a pivotal role in the Quantum Language compiler's pipeline. It takes the tokenized output from the lexer as input and constructs a structured AST that represents the program's syntax. The AST serves as the foundation for subsequent stages such as semantic analysis, optimization, and code generation.

### Key Design Decisions and Why

1. **Token Stream Management**: The parser maintains a stream of tokens to facilitate efficient parsing. This involves keeping track of the current position (`pos`) within the token list.
   
2. **Lookahead Mechanism**: To handle complex grammatical structures, the parser uses a lookahead mechanism. The `peek` function allows the parser to inspect upcoming tokens without advancing the position, enabling it to make informed decisions about the next steps in parsing.

3. **Error Handling**: Robust error handling is essential for maintaining user-friendly feedback during compilation. The `expect` function throws a `ParseError` when the expected token type does not match the current token, providing clear context and guidance on how to correct the issue.

4. **Flexibility with Statements**: The parser supports various types of statements, including expressions, declarations, and control flow statements. Each statement type is handled by its respective parsing function, ensuring flexibility and scalability.

5. **Skip Newlines**: The `skipNewlines` function facilitates the removal of unnecessary newline tokens, improving readability and reducing clutter in the AST.

## Major Classes/Functions Overview

### Parser Class

- **Constructor**: Initializes the parser with a vector of tokens.
- **Current Token**: Returns the current token being processed.
- **Peek Token**: Allows inspection of the token at a specified offset without advancing the position.
- **Consume Token**: Advances the position to the next token and returns the current one.
- **Expect Token**: Checks if the current token matches the expected type; if not, throws a `ParseError`.
- **Check Token**: Verifies if the current token matches a specific type.
- **Match Token**: Attempts to match and consume a token of a specified type; returns `true` if successful.
- **At End**: Determines if the parser has reached the end of the token stream.
- **Skip Newlines**: Removes consecutive newline tokens from the stream.
- **Parse**: Main entry point for parsing the entire token stream into an AST.

### ASTNode Class

Represents nodes in the Abstract Syntax Tree. Each node can be of different types, such as statements, expressions, or declarations. The `ASTNode` class encapsulates these types and provides methods for accessing and manipulating their properties.

### BlockStmt Class

A specialized AST node representing a block of statements. It is used to group multiple statements together, often found in compound statements like loops or conditionals.

## Tradeoffs

1. **Complexity vs. Readability**: While the lookahead mechanism enhances the parser's ability to handle complex grammars, it also increases complexity and potential for errors. Balancing between robustness and simplicity is a continuous challenge.

2. **Performance**: Efficiently managing the token stream and performing lookahead operations require careful consideration of performance. Optimizing data structures and algorithms can help mitigate potential bottlenecks.

3. **Error Reporting**: Providing clear and actionable error messages requires a nuanced understanding of the grammar and common pitfalls. Overly complex error reporting mechanisms can obscure the actual issues, leading to frustration for users.

4. **Scalability**: Supporting a wide range of statement types and extending the language's syntax in the future necessitate a flexible and extensible parser architecture. Ensuring that new features can be added without breaking existing functionality is crucial.

By addressing these tradeoffs, the `ParserCore.cpp` aims to provide a reliable and scalable foundation for the Quantum Language compiler's parsing stage.