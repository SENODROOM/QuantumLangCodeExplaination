# `check` Function

## Overview

The `check` function serves as a pivotal element in the Quantum Language compiler's type-checking phase. Its primary role is to validate that the source code conforms to the language's syntactic and semantic guidelines. The function employs a recursive approach to navigate through the abstract syntax tree (AST) of the program, ensuring each part adheres to the defined rules.

## Parameters

- **`node`**: A pointer to the current AST node being evaluated. This parameter can be any type of node, including blocks, expressions, statements, etc.
- **`globalEnv`**: A reference to the global environment object, which holds information about variables, functions, and types declared at the global scope. This environment is essential for resolving identifiers and validating their usage.

## Return Value

The `check` function does not return any value explicitly (`void`). However, its execution ensures that all nodes within the AST are properly validated, and any errors or inconsistencies are reported back to the user.

## Edge Cases

1. **Null Node**: If the input `node` is `nullptr`, the function should handle this case gracefully without causing runtime errors.
2. **Empty Block Statement**: When encountering an empty block statement (`BlockStmt`), the function should simply proceed without performing any checks on an empty list of statements.

## Interactions with Other Components

The `check` function interacts closely with several other components within the compiler:

1. **Symbol Table**: It uses the symbol table to look up identifiers and ensure they have been declared before use.
2. **Type System**: The function leverages the type system to verify that operations and assignments are performed between compatible data types.
3. **Error Reporting**: Upon detecting any issues during the type-checking process, the `check` function reports these errors using the error reporting mechanism provided by the compiler.

Here's how the function handles different types of AST nodes:

- **Block Statements**: For block statements, the function calls itself recursively to check each individual statement within the block. This ensures that nested structures are also validated correctly.
  
  ```cpp
  if (node && node->is<BlockStmt>()) {
      check(node->as<BlockStmt>().statements);
  }
  ```

- **Other Nodes**: For all other types of nodes (expressions, statements, etc.), the function calls `checkNode`, passing the current node and the global environment. This method is responsible for performing the actual validation checks based on the node type.

  ```cpp
  else {
      checkNode(node, globalEnv);
  }
  ```

In summary, the `check` function plays a critical role in maintaining the integrity and correctness of the Quantum Language source code. By employing a recursive strategy and interacting with various components such as the symbol table and type system, it ensures that all parts of the program adhere to the language's rules, thereby facilitating efficient compilation and execution.