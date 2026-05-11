# TypeChecker Component

The TypeChecker component is an essential part of the Quantum Language compiler, tasked with executing static type checking on the Abstract Syntax Tree (AST). This step validates that the code adheres strictly to predefined type rules, thus preventing runtime type-related errors before execution commences.

## Overview

The primary function of the TypeChecker component is to ensure type safety in the quantum programming language. It traverses the AST and checks the types of variables, expressions, and statements against the defined type system. The component consists of several key functions:

- `check`: Main entry point for type checking.
- `checkNode`: Recursively checks individual nodes within the AST.
- `checkFunction`: Validates the types of function parameters and return values.
- `checkVariable`: Ensures variables are declared and used correctly with their specified types.
- `checkExpression`: Checks the types of expressions and performs necessary type conversions.

These functions work together to create a comprehensive type-checking mechanism that covers all aspects of the quantum language's syntax and semantics.

## Files and Their Roles

### `type_checker.h`

This header file declares the public interface of the TypeChecker component. It includes function prototypes for `check`, `checkNode`, and other related functions. This file serves as the contract between the compiler and the type checker, ensuring that the type checker can be invoked correctly and that its output is properly handled.

### `type_checker.cpp`

This source file implements the functionality declared in `type_checker.h`. It contains the definitions of the `check` and `checkNode` functions, along with helper functions like `checkFunction`, `checkVariable`, and `checkExpression`. These implementations are crucial for the actual type checking process, where the AST is analyzed and validated against the language's type rules.

### `type_system.h`

This header file defines the type system used by the TypeChecker component. It includes declarations for various data types, such as qubits, complex numbers, and custom user-defined types. The type system is fundamental to the type checking process, as it determines the valid operations and interactions between different types.

### `type_system.cpp`

This source file provides the implementation details for the type system defined in `type_system.h`. It includes functions for creating, manipulating, and querying types, as well as handling type conversions and promotions. The type system implementation ensures that the type checker has access to accurate and complete information about the types being used in the quantum program.

## Overall Flow

1. **Initialization**: The TypeChecker component initializes itself and sets up any necessary data structures or configurations.
2. **Traversal**: The `check` function is called with the root node of the AST. It then recursively calls `checkNode` on each node in the tree.
3. **Type Checking**: For each node, `checkNode` determines the appropriate type checking function to call based on the node's type (e.g., variable declaration, function call).
4. **Validation**: The selected type checking function validates the node according to the language's type rules. If any issues are found, they are reported as errors.
5. **Completion**: Once the entire AST has been checked, the TypeChecker component reports any accumulated errors and completes the type checking process.

By following this structured approach, the TypeChecker component ensures thorough and accurate type validation, contributing significantly to the reliability and correctness of compiled quantum programs.