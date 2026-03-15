# TypeChecker Component

The TypeChecker component is an integral part of the Quantum Language compiler, designed to perform static type checking on the Abstract Syntax Tree (AST). This ensures that the code adheres to the defined type rules, preventing potential type-related errors at runtime.

## Overview

The primary function of the TypeChecker is to traverse the AST and verify the types of variables, expressions, and statements. It uses a type environment (`globalEnv`) to keep track of variable types throughout the compilation process. The component includes several key functions:

- `check`: Main entry point for type checking a program.
- `checkNode`: Recursively checks individual AST nodes.
- `check`: Specialized check function for different node types.

## Files and Their Roles

### TypeChecker.h
- **Purpose**: Declares the public interface of the TypeChecker class, including its methods and data members.
- **Contents**:
  - Class definition for `TypeChecker`.
  - Declaration of the `globalEnv` member variable.
  - Function prototypes for `check`, `checkNode`, and specialized check functions.

### TypeChecker.cpp
- **Purpose**: Implements the functionality declared in `TypeChecker.h`. Contains the logic for traversing the AST and performing type analysis.
- **Contents**:
  - Constructor for `TypeChecker`, initializing the `globalEnv`.
  - Implementation of the `check` method, which initiates the type checking process.
  - Implementation of the `checkNode` method, which recursively checks each node in the AST.
  - Specialized implementations of the `check` method for various node types, such as variables, operations, and control structures.

## Overall Flow

1. **Initialization**: A new instance of `TypeChecker` is created, setting up the initial type environment.
2. **Traversal**: The `check` method is called with the root of the AST. This triggers a recursive traversal of all nodes in the tree.
3. **Type Analysis**: During traversal, the `checkNode` method evaluates the type of each node based on its structure and the current state of the type environment.
4. **Error Handling**: If any type inconsistencies or errors are detected, they are reported using the `Error` class.
5. **Completion**: After all nodes have been checked, the type checker concludes, ensuring that the entire program is type-consistent.

## Dependencies

- `TypeEnv`: Manages the type information for variables and expressions.
- `Error`: Handles reporting of type-related errors.

## Usage

To use the TypeChecker component, include it in your project and call the `check` method with the root of your AST. The component will then analyze the types and report any issues found.

```cpp
#include "TypeChecker.h"

int main() {
    // Create AST and populate it with nodes
    std::shared_ptr<ASTNode> rootNode = ...;

    // Initialize TypeChecker
    TypeChecker tc;

    // Perform type checking
    tc.check(rootNode);

    return 0;
}
```

By integrating the TypeChecker into your Quantum Language compiler, you can enhance the robustness of your compiler by catching type errors early in the development process.