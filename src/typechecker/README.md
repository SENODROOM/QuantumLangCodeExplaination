# TypeChecker Component

The TypeChecker component is an essential part of the Quantum Language compiler, responsible for performing static type checking on the Abstract Syntax Tree (AST). This process helps ensure that the code complies with the specified type rules, thereby avoiding runtime type-related errors.

## Overview

The main objective of the TypeChecker is to traverse the AST and validate the types of variables, expressions, and statements. It employs a recursive approach to inspect each node in the AST and apply type inference algorithms based on the language's syntax and semantics.

### Files and Their Roles

- **`check.cpp`**: Contains the core logic for type checking. This file includes the implementation of the `TypeChecker` class and its methods for traversing and validating nodes.
- **`check.h`**: Defines the interface for the type checker. This header file declares the public methods and data structures used by the `TypeChecker` class.
- **`checkNode.cpp`**: Implements specific checks for different types of AST nodes such as declarations, assignments, and function calls. Each node type has its own validation method within this file.

### Overall Flow

1. **Initialization**: The `TypeChecker` object is initialized with the root of the AST.
2. **Traversal**: The type checker recursively traverses through each node in the AST.
3. **Validation**: For each node, the appropriate validation method is called to check the type correctness according to the quantum language rules.
4. **Error Handling**: If any type error is detected during traversal, it is reported and the compilation process may be halted or continue with warnings depending on the severity of the error.
5. **Completion**: Once all nodes have been checked, the type checker concludes the type checking phase and provides a report indicating whether the code passed all type validations.

By ensuring that the AST accurately reflects the intended types of quantum operations and variables, the TypeChecker significantly enhances the reliability and performance of the compiled quantum programs.