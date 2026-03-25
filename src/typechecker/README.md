# TypeChecker Component

The TypeChecker component is a crucial part of the Quantum Language compiler, designed to perform static type checking on the Abstract Syntax Tree (AST). This ensures that the code adheres to the defined type rules, preventing runtime type-related errors.

## Overview

The primary function of the TypeChecker is to traverse the AST and validate the types of variables, expressions, and statements according to the language's type system. The `check` function serves as the entry point for initiating the type checking process. It calls `checkNode`, which recursively checks each node in the AST. These checks include verifying variable declarations, function definitions, and usage, ensuring that all operations are performed between compatible types.

## Files and Functions

### check.cpp

- **Function:** `void check(ASTNode* root)`
  - **Description:** This function is the starting point for the type checking process. It initializes the type checking environment and begins the traversal of the AST from the root node.

### check.h

- **Function:** `bool checkNode(ASTNode* node)`
  - **Description:** Recursively checks each node in the AST. Depending on the type of node, it performs specific type validation checks such as variable type consistency, function argument matching, and return type verification.

### checkUtils.cpp

- **Function:** Various utility functions used throughout the type checking process, including type resolution, type inference, and error reporting.

### checkUtils.h

- **Header File:** Contains declarations for the utility functions used in type checking.

## Flow Diagram

```
+-------------------+
|    check()        |
+---------+---------+
          |
          v
+---------+---------+
| checkNode()       |
+---------+---------+
          |
          v
+---------+---------+
| Variable Check    |
+---------+---------+
          |
          v
+---------+---------+
| Function Check    |
+---------+---------+
          |
          v
+---------+---------+
| Expression Check  |
+---------+---------+
          |
          v
+---------+---------+
| Error Reporting   |
+-------------------+
```

## Usage

To use the TypeChecker component, compile your quantum program using the Quantum Language compiler. During compilation, the TypeChecker will automatically run and report any type errors found in the AST.

## Contributing

Contributions to the TypeChecker component are welcome! If you find a bug or have an enhancement request, please open an issue on our GitHub repository. For more detailed information on contributing, refer to the [CONTRIBUTING.md](../CONTRIBUTING.md) file.

## License

The TypeChecker component is licensed under the MIT License. See the [LICENSE](../LICENSE) file for details.