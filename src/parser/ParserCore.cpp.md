# ParserCore.cpp

## Overview

`ParserCore.cpp` is a critical component of the Quantum Language compiler, responsible for converting the input source code into an Abstract Syntax Tree (AST). This file contains the implementation of the `Parser` class, which manages both the lexical analysis and syntactic parsing phases of the compilation process.

## Role in Compiler Pipeline

The parser plays a pivotal role in the Quantum Language compiler's pipeline. It takes the tokenized input produced by the lexer and constructs a structured AST that represents the program's syntax. This AST serves as the foundation for subsequent stages such as semantic analysis, optimization, and code generation.

### Key Design Decisions and Why

1. **Token Stream Management**: The `Parser` class maintains a stream of tokens (`tokens`) and a position pointer (`pos`). This allows the parser to efficiently traverse and consume tokens as it builds the AST.

2. **Error Handling**: The `Parser` throws exceptions when encountering unexpected tokens or syntax errors. This ensures that the compiler can provide clear and actionable error messages to the user, facilitating easier debugging and correction of issues in the source code.

3. **Syntactic Parsing Rules**: The parser uses recursive descent parsing to implement the language's grammar rules. This approach simplifies the implementation and makes it easier to understand the structure of the language.

4. **Skip Newlines Functionality**: The `skipNewlines` function is designed to handle multiple consecutive newline characters gracefully. This ensures that the parser does not treat them as separate statements, maintaining the integrity of the program's flow control.

5. **Expression Parsing**: The parser includes specialized functions for parsing different types of expressions, such as literals, variables, and arithmetic operations. These functions ensure that the parser correctly handles operator precedence and associativity.

6. **Statement Parsing**: The parser supports various statement types, including declarations, assignments, and control structures like loops and conditionals. Each statement type has its own parsing function, allowing the parser to build a comprehensive AST.

7. **Block Parsing**: The parser handles blocks of statements, ensuring that they are grouped together correctly. This is essential for maintaining the scope and context of variables within the program.

8. **Lookahead Mechanism**: The `peek` function provides lookahead capabilities, enabling the parser to anticipate upcoming tokens without consuming them. This helps in making decisions based on future syntax, such as determining whether a statement is followed by another statement or a declaration.

9. **Early Error Detection**: By checking for specific token types before attempting to parse a statement, the parser can detect potential errors early in the parsing process. This prevents further processing of invalid syntax and reduces the complexity of error handling later on.

10. **Modular Architecture**: The parser is designed with a modular architecture, where each part of the grammar is encapsulated in its own function. This makes the code more readable, maintainable, and extensible.

## Major Classes/Functions Overview

- **Parser Class**:
  - **Constructor**: Initializes the parser with a vector of tokens.
  - **Current Token**: Returns the current token being processed.
  - **Peek Token**: Returns the token at a specified offset without advancing the position pointer.
  - **Consume Token**: Advances the position pointer and returns the current token.
  - **Expect Token**: Consumes the next token if it matches the expected type, otherwise throws a `ParseError`.
  - **Check Token**: Checks if the current token matches a specified type.
  - **Match Token**: Attempts to match the current token against a specified type and consumes it if successful.
  - **At End**: Determines if the end of the token stream has been reached.
  - **Skip Newlines**: Skips over any consecutive newline characters.
  - **Parse Function**: Main entry point for parsing the entire input source code, constructing an AST representing the program.

- **ASTNode Class**:
  - Represents nodes in the Abstract Syntax Tree.
  - Contains information about the node's type, value, and location in the source code.

- **Statement Classes**:
  - **BlockStmt**: Represents a block of statements.
  - **DeclarationStmt**: Represents variable declarations.
  - **AssignmentStmt**: Represents assignment statements.
  - **ControlFlowStmt**: Represents control flow statements like loops and conditionals.

- **Expression Classes**:
  - **LiteralExpr**: Represents literal values.
  - **VariableExpr**: Represents variable references.
  - **ArithmeticExpr**: Represents arithmetic operations.

## Tradeoffs

- **Performance vs. Readability**: While recursive descent parsing is straightforward and easy to implement, it can lead to less efficient code due to repeated function calls. To mitigate this, the parser employs techniques such as memoization and lookahead caching to optimize performance.

- **Flexibility vs. Complexity**: A highly flexible parser that can handle complex grammars may become more difficult to maintain and debug. By adhering to a well-defined grammar and using modular functions, the parser achieves a balance between flexibility and simplicity.

- **Error Handling Precision vs. Usability**: Early error detection can improve the precision of error messages but may also increase the complexity of the parser. By providing clear and actionable error messages, the parser enhances usability and aids in effective debugging.

- **Memory Usage vs. Parsing Speed**: Storing the entire token stream in memory can be resource-intensive, especially for large programs. However, this approach allows for efficient lookahead and consumption of tokens, optimizing parsing speed.

In conclusion, `ParserCore.cpp` is a vital component of the Quantum Language compiler, playing a crucial role in transforming the input source code into a structured AST. Through careful design and implementation, the parser ensures robustness, efficiency, and ease of maintenance, forming the backbone of the compiler's functionality.