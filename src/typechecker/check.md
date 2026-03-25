# `check` Function

## Overview

The `check` function serves as a central component in the Quantum Language compiler's type checking phase. Its primary role is to validate the syntax and semantics of the source code against the language's defined rules and constraints. The function processes an abstract syntax tree (AST) node and recursively checks its children nodes to ensure they adhere to the language's type system.

### Why It Works This Way

The current implementation of the `check` function handles two types of AST nodes: `BlockStmt` and other node types. For `BlockStmt`, it recursively calls itself on each statement within the block. This approach ensures that all statements within a block are checked individually, maintaining the integrity of the block structure.

For other node types, it directly calls the `checkNode` function, passing the node and the global environment (`globalEnv`). This allows for specialized type checking based on the node's type, leveraging the existing type checking logic for different constructs.

This design allows the `check` function to be flexible and handle various types of AST nodes while ensuring thorough type validation throughout the entire program.

## Parameters

- **`node`:** A pointer to an AST node representing the current node being processed.
- **`globalEnv`:** A reference to the global environment object, which contains information about the program's scope and variables.

## Return Value

The `check` function does not return any value explicitly. Instead, it performs type checking operations and may throw exceptions or generate error messages if type violations are detected.

## Edge Cases

1. **Null Node:** If the input `node` is null, the function should gracefully handle this case without performing any checks.
2. **Empty Block:** If a `BlockStmt` contains no statements, the function should still perform necessary checks to ensure the block is valid.
3. **Nested Blocks:** The function should correctly handle nested blocks, ensuring that each statement within every block is checked.

## Interactions with Other Components

- **`TypeChecker` Class:** The `check` function is part of the `TypeChecker` class, which manages the overall type checking process.
- **`checkNode` Function:** The `check` function interacts with the `checkNode` function, which provides specialized type checking for different AST node types.
- **Global Environment:** The `check` function uses the `globalEnv` parameter to access information about the program's scope and variables, allowing for context-aware type checking.

By interacting with these components, the `check` function ensures comprehensive type validation across the entire AST, contributing to the reliability and correctness of the compiled quantum programs.