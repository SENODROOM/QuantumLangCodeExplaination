# `check` Function

## Overview

The `check` function serves as a central component in the Quantum Language compiler's type checking phase. Its primary role is to validate the syntax and semantics of the source code against the language's defined rules and constraints. The function processes an abstract syntax tree (AST) node and recursively checks its children nodes to ensure they adhere to the language's type system.

### Why It Works This Way

The `check` function operates on AST nodes and uses recursion to traverse the entire tree. This approach ensures that all parts of the source code are checked, including nested blocks and expressions. By first checking if the node is a `BlockStmt`, the function can handle blocks of statements separately, applying block-level rules before moving on to individual nodes within the block.

If the node is not a `BlockStmt`, the function calls `checkNode`, which performs more granular checks tailored to the specific type of node. This separation allows for efficient handling of different node types and their respective rules, making the type checking process both comprehensive and modular.

### Parameters

- **`node`**: A pointer to the current AST node being checked. If `nullptr`, the function returns immediately without performing any checks.
- **`globalEnv`**: A reference to the global environment, which contains information about the language's built-in types, functions, and variables. This parameter is used by `checkNode` to resolve identifiers and apply type constraints.

### Return Value

The `check` function does not return any value explicitly. Instead, it raises exceptions or errors if any type-related issues are detected during the checking process. These exceptions can then be handled by higher-level components of the compiler, such as error reporting or recovery mechanisms.

### Edge Cases

1. **Empty Block Statements**: If a `BlockStmt` contains no statements, the function will still proceed to check the block itself, ensuring that any block-level rules are applied correctly.
2. **Null Nodes**: Passing a `nullptr` to the `check` function results in immediate termination without any action, preventing potential null pointer dereferences.
3. **Unknown Node Types**: While the function primarily handles known node types through `checkNode`, unexpected or custom node types might require additional handling or validation logic.

### Interactions With Other Components

- **Error Reporting**: The `check` function interacts with the error reporting mechanism to provide detailed feedback on type-related errors. This includes generating appropriate error messages and highlighting problematic areas in the source code.
- **Symbol Table Management**: During the checking process, the function updates and queries the symbol table managed by the global environment (`globalEnv`). This helps in resolving identifiers and validating their types.
- **Code Generation**: Although not directly involved in type checking, the `check` function indirectly supports code generation by ensuring that only valid and well-typed code is processed. This prevents runtime errors related to type mismatches or undefined behavior.

By leveraging recursion and specialized checks for different node types, the `check` function provides a robust and scalable solution for type validation in the Quantum Language compiler. This ensures that the compiled code adheres strictly to the language's type system, reducing the likelihood of runtime errors and improving overall program reliability.