# `check` Function

## Overview

The `check` function is a crucial component of the Quantum Language compiler's type-checking phase. Its main purpose is to ensure that the source code adheres to the language's syntactic and semantic rules. The function uses a recursive approach to traverse the abstract syntax tree (AST) and perform type checking on each node.

### Why It Works This Way

The recursive nature of the `check` function allows it to handle nested structures within the AST efficiently. By breaking down the problem into smaller subproblems, it ensures that all parts of the code are checked comprehensively. This method also facilitates error handling at multiple levels, making it easier to pinpoint issues in complex programs.

## Parameters

- **`node`**: A pointer to the current AST node being processed. If the node is a block statement (`BlockStmt`), the function will recursively check its statements.
- **`globalEnv`**: A reference to the global environment, which contains information about types, variables, and functions available throughout the program.

## Return Value

The `check` function does not return any value explicitly. Instead, it performs type checking and raises errors or warnings when necessary. If an error is detected, the compilation process is halted, and an appropriate error message is displayed.

## Edge Cases

1. **Empty Block Statement**: If the node is a block statement but contains no statements, the function should proceed without raising an error.
2. **Null Node Pointer**: If the `node` parameter is `nullptr`, the function should handle this case gracefully, possibly indicating an unexpected condition during the traversal.
3. **Unsupported Node Types**: If the node type is not recognized or supported by the type checker, the function should raise an error indicating that the node type is invalid.

## Interactions With Other Components

The `check` function interacts closely with several other components of the Quantum Language compiler:

1. **Symbol Table**: The global environment (`globalEnv`) acts as a symbol table, storing information about identifiers such as variables, functions, and types. The `check` function uses this information to resolve references and enforce type consistency.
2. **Error Reporting**: When an error is encountered during type checking, the `check` function reports it using the compiler's error reporting mechanism. This helps developers understand where and why their code violates the language's rules.
3. **Scope Management**: The function manages scopes by keeping track of the current context. For example, when entering a new block statement, the scope is updated to include the block's local variables. This ensures that type checking respects variable visibility and scoping rules.

By leveraging these interactions, the `check` function provides robust type checking capabilities, helping to catch and fix errors early in the development process.