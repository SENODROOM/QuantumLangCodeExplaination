# ParserCore.cpp

## Overview

`ParserCore.cpp` is a crucial component of the Quantum Language compiler, responsible for transforming the input source code into an Abstract Syntax Tree (AST). This file contains the implementation of the `Parser` class, which manages both the lexical analysis and syntactic parsing stages of the compilation process.

## Role in Compiler Pipeline

The parser operates as the second stage in the compiler's pipeline after lexical analysis. Its primary responsibility is to convert the sequence of tokens produced by the lexer into a structured AST that represents the program's syntax. This AST serves as the foundation for subsequent phases such as semantic analysis, optimization, and code generation.

### Key Design Decisions and Why

1. **Separation of Lexical Analysis and Syntactic Parsing**: By separating these two stages, the parser ensures that it can operate on a clean stream of tokens without worrying about tokenization details. This separation enhances modularity and maintainability.

2. **Abstract Syntax Tree (AST)**: The use of an AST allows the parser to represent complex structures like nested blocks, function calls, and control flow statements in a hierarchical manner. This representation facilitates easier manipulation and analysis during later stages of compilation.

3. **Exception Handling for Errors**: The parser includes robust error handling mechanisms to manage unexpected tokens or syntax errors. These exceptions provide detailed information about the error location and type, aiding in debugging and improving user experience.

4. **Flexibility with Token Consumption**: The parser provides methods to consume tokens (`consume`) and check their types (`check`, `match`). This flexibility enables precise control over how tokens are processed and helps in building complex grammars.

## Major Classes/Functions Overview

### Parser Class

- **Constructor**: Initializes the parser with a vector of tokens.
- **Current Method**: Returns the current token being processed.
- **Peek Method**: Returns the token at a specified offset ahead in the token stream.
- **Consume Method**: Advances the position in the token stream and returns the current token.
- **Expect Method**: Checks if the current token matches the expected type and throws an exception if not, otherwise advancing the position.
- **Check Method**: Verifies if the current token has the specified type.
- **Match Method**: Attempts to match the current token with the specified type and advances the position if successful.
- **AtEnd Method**: Determines if the end of the token stream has been reached.
- **SkipNewlines Method**: Skips any consecutive newline tokens.
- **Parse Method**: Main entry point for parsing the entire source code. It constructs a block statement containing all parsed statements.

### ASTNode Class

- Represents nodes in the Abstract Syntax Tree.
- Contains various types of statements, expressions, and declarations.
- Provides methods to access and manipulate the tree structure.

### BlockStmt Class

- Inherits from ASTNode.
- Represents a block of statements, typically used for grouping code within functions, loops, or conditionals.
- Contains a list of statements.

## Tradeoffs

1. **Complexity vs. Simplicity**:
   - **Complexity**: The parser requires careful handling of different token types and grammar rules, leading to more complex code.
   - **Simplicity**: A simpler parser might miss important features or be harder to extend.

2. **Performance vs. Usability**:
   - **Performance**: Efficient parsing algorithms can improve overall compilation speed.
   - **Usability**: Detailed error messages and flexible token consumption enhance usability and debugging capabilities.

3. **Memory Usage vs. Execution Time**:
   - **Memory Usage**: Storing the entire AST in memory can increase memory usage, especially for large programs.
   - **Execution Time**: Optimized AST traversal and manipulation can reduce execution time during subsequent phases.

By carefully balancing these tradeoffs, the `ParserCore.cpp` aims to provide a reliable and efficient tool for converting Quantum Language source code into an AST, supporting the broader goals of the compiler.