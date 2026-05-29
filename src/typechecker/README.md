# TypeChecker Component

The TypeChecker component is a vital part of the Quantum Language compiler, tasked with executing static type validation on the Abstract Syntax Tree (AST). This process helps ensure that the code adheres to predefined type constraints, thus preventing runtime errors related to type mismatches before execution starts.

## Overview

The primary function of the TypeChecker component is to traverse and analyze the AST to verify the types of variables, expressions, and statements. It uses a combination of built-in type definitions and user-defined type declarations to enforce type safety throughout the compilation process.

### Files and Their Roles

- **`check.cpp`**: Contains the core logic for type checking. The `checkNode` function is central to this file, as it processes each node in the AST according to its type.
  
- **`check.h`**: Declares the public interface for the type checker, including the `checkNode` function prototype and any necessary data structures or enums to represent different types and their relationships.

- **`type_checker.cpp`**: Implements additional functionality specific to the type checker, such as handling custom type definitions, type inference, and error reporting.

- **`type_checker.h`**: Exposes the implementation details of the type checker, allowing other parts of the compiler to interact with it effectively.

## Flow of Execution

1. **Initialization**: The TypeChecker component initializes itself by setting up any required data structures or parsing user-defined type declarations from the source code.

2. **Traversal**: The component traverses the AST using a depth-first search (DFS) approach. Each node in the AST is processed by the `checkNode` function, which determines the appropriate type checking based on the node's type.

3. **Type Validation**: For each node, the `checkNode` function validates the type against the expected type. If there is a mismatch, an error message is generated indicating the issue.

4. **Error Reporting**: Errors detected during type checking are reported back to the compiler, providing detailed information about the location and nature of the problem.

5. **Completion**: Once all nodes have been checked, the TypeChecker component completes its task, ensuring that the entire AST has been validated without any type-related issues.

By following this structured approach, the TypeChecker component plays a critical role in maintaining the integrity and reliability of the compiled quantum programs.