# ParserCore.cpp

## Overview

`ParserCore.cpp` is a critical component of the Quantum Language compiler, responsible for converting the input source code into an Abstract Syntax Tree (AST). This file houses the implementation of the `Parser` class, which orchestrates both the lexical analysis and syntactic parsing phases during the compilation process.

## Role in Compiler Pipeline

The parser operates as the second stage in the compiler's pipeline after lexical analysis. Its primary responsibility is to take the sequence of tokens produced by the lexer and construct a structured AST that represents the syntax of the quantum program. This AST serves as the foundation for subsequent semantic analysis, optimization, and code generation stages.

### Key Design Decisions and Why

1. **Separation of Lexical Analysis and Syntactic Parsing**:
   - The parser relies on pre-processed tokens provided by the lexer. This separation ensures that the parser can focus solely on understanding the structure of the language without being concerned with tokenization details.

2. **Use of Token Streams**:
   - The parser processes tokens using a stream-like interface (`tokens` vector and `pos` pointer). This approach allows efficient traversal and consumption of tokens, making it easier to handle complex grammatical structures.

3. **Error Handling**:
   - The parser includes robust error handling mechanisms through the `ParseError` exception. When a syntax error is encountered, it throws an exception with a descriptive message, line number, and column position, aiding in debugging and improving user experience.

4. **Flexibility and Extensibility**:
   - The parser is designed to be flexible and extensible. It supports easy addition of new grammar rules and token types by modifying the existing codebase or adding new methods.

## Major Classes/Functions Overview

### Parser Class

- **Constructor**: Initializes the parser with a vector of tokens.
- **Current Method**: Returns the current token being processed.
- **Peek Method**: Allows looking ahead at a specified offset in the token stream.
- **Consume Method**: Advances the token stream to the next token.
- **Expect Method**: Consumes the current token if its type matches the expected type, otherwise throws a `ParseError`.
- **Check Method**: Checks if the current token has the specified type.
- **Match Method**: Consumes the current token if its type matches the specified type, returning `true` if successful.
- **AtEnd Method**: Determines if the end of the token stream has been reached.
- **SkipNewlines Method**: Skips over any newline tokens in the stream.
- **Parse Method**: Constructs the AST by repeatedly calling `parseStatement()` until the end of the token stream is reached.

### ASTNode Class

- Represents a node in the Abstract Syntax Tree.
- Contains various types of statements such as `BlockStmt`, `ExpressionStmt`, etc.
- Each statement type has specific attributes and behaviors defined within its respective subclass.

### BlockStmt Class

- Inherits from `ASTNode`.
- Represents a block of statements, typically used in control flow constructs like loops and conditionals.
- Stores a list of statements (`statements`) and provides methods to manipulate this list.

## Tradeoffs

1. **Complexity vs. Flexibility**:
   - While the parser is designed to be flexible and extensible, this flexibility comes at the cost of increased complexity. Adding new grammar rules requires careful consideration to maintain the integrity and correctness of the AST construction logic.

2. **Performance vs. Error Handling**:
   - The parser prioritizes thorough error handling to ensure that all syntax errors are caught and reported accurately. However, this can sometimes lead to performance overhead due to additional checks and exception handling.

3. **Code Readability vs. Efficiency**:
   - The parser aims to provide clear and readable code by using intuitive method names and explicit state management. This readability can sometimes sacrifice efficiency, especially when dealing with large token streams or complex grammatical structures.

Overall, `ParserCore.cpp` plays a vital role in the Quantum Language compiler by translating the abstract syntax of the source code into a structured AST. Its design choices balance flexibility, performance, and error handling, ensuring that the compiler can effectively parse and understand quantum programs.