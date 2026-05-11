# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` is an essential component of the Quantum Language compiler, tasked with performing static type checking on the Abstract Syntax Tree (AST). This process ensures that all expressions and statements conform to their declared types, thus preventing runtime errors caused by type mismatches.

## Role in the Compiler Pipeline

The `TypeChecker` plays a pivotal role in the compiler's pipeline by integrating itself after the parser has generated the AST. Its primary function is to traverse the AST and verify the types of variables, literals, and expressions according to the language's type system. If any inconsistencies are found, it issues warnings or errors to help developers correct their code before execution.

## Key Design Decisions and Why

1. **Global Environment**: The `TypeChecker` maintains a global environment (`globalEnv`) to store information about predefined functions and variables. This allows the checker to resolve types for built-in functions and variables efficiently.

2. **Recursive Checking**: The `check` method is designed to handle both individual nodes and blocks of statements recursively. This ensures that nested structures within the AST can also be checked for type correctness.

3. **Type Inference**: For variable declarations without explicit type hints, the `TypeChecker` infers the type based on the initializer expression. This approach simplifies the development of type-safe code while reducing boilerplate.

4. **Error Handling**: The `checkNode` method uses error handling mechanisms to report type-related issues. By issuing warnings and errors, the compiler helps developers identify and fix potential problems early in the development cycle.

## Major Classes/Functions Overview

### Class: `TypeChecker`

- **Constructor**: Initializes the global environment with predefined functions and variables.
- **Method: `check(const std::vector<ASTNodePtr>& nodes)`**
  - Iterates through each node in the provided vector and checks its type using the `checkNode` method.
- **Method: `check(const ASTNodePtr& node)`**
  - Handles individual nodes and calls `checkNode` to perform type checking.
  
### Function: `checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)`

- **Purpose**: Recursively checks the type of a given AST node within a specified environment.
- **Handling Different Node Types**:
  - **NumberLiteral**: Returns `"float"` since all numbers are treated as floating-point values.
  - **StringLiteral**: Returns `"string"` as these are literal string values.
  - **BoolLiteral**: Returns `"bool"` for boolean literals.
  - **Identifier**: Resolves the identifier's type from the current environment.
  - **VarDecl**: Checks the type of the variable declaration against its initializer and updates the environment accordingly.
  - **FunctionDecl**: Sets up a new environment for the function parameters and checks the body of the function.
  - **BlockStmt**: Creates a new environment for the block and iteratively checks each statement within it.
  - **BinaryExpr**: Checks the types of both operands and determines the result type based on the operator used.

## Tradeoffs

1. **Complexity vs. Usability**: While providing robust type checking enhances code safety, it adds complexity to the compiler implementation. Balancing thoroughness with usability is crucial for developer productivity.

2. **Performance**: Recursive type checking can lead to performance overhead, especially for large ASTs. Optimizing the checking algorithm to reduce computational complexity is necessary for efficient compilation.

3. **Flexibility vs. Strictness**: Allowing type inference provides flexibility but may lead to subtle bugs if not handled carefully. Striking a balance between strict adherence to declared types and allowing reasonable type flexibility is important for practical use.

By addressing these tradeoffs, the `TypeChecker.cpp` aims to provide a reliable and efficient mechanism for static type checking in the Quantum Language compiler.