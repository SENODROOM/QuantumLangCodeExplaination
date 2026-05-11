# `check` Function

## Overview

The `check` function is a crucial component within the Quantum Language compiler's type checking phase. Its main objective is to ensure that the source code adheres to the language's syntactic and semantic rules. This function operates recursively to traverse through the abstract syntax tree (AST) of the program, examining each node for compliance with the specified constraints.

## Parameters

- **`node`**: A pointer to the current AST node being processed. If the node is a `BlockStmt`, the function will process its contained statements.
- **`globalEnv`**: A reference to the global environment object, which holds information about the program's scope, variables, and types.

## Return Value

The function does not return any value (`void`). Instead, it performs the necessary checks and may raise errors or exceptions if the code violates the language's rules.

## Edge Cases

1. **Null Node**: If the input `node` is `nullptr`, the function should handle this gracefully without causing undefined behavior.
2. **Non-Block Statement**: If the `node` is not a `BlockStmt`, the function directly calls `checkNode` on the given node. This ensures that individual nodes are also validated according to their respective rules.

## How It Works

The function starts by checking if the provided `node` is not `nullptr`. If it is a `BlockStmt`, it proceeds to check all the statements contained within the block. Otherwise, it calls the helper function `checkNode` to validate the individual node.

### Detailed Steps

1. **Check Null Node**:
   ```cpp
   if (!node) {
       // Handle null node case
       return;
   }
   ```

2. **Process Block Statements**:
   ```cpp
   if (node->is<BlockStmt>()) {
       auto block = node->as<BlockStmt>();
       for (auto stmt : block.statements) {
           check(stmt);  // Recursively check each statement in the block
       }
   }
   ```

3. **Validate Individual Nodes**:
   ```cpp
   else {
       checkNode(node, globalEnv);  // Validate the single node using the global environment
   }
   ```

## Interactions with Other Components

- **Type Checker**: The `check` function interacts closely with the type checker to validate variable declarations, expressions, and control flow constructs.
- **Global Environment**: The `globalEnv` parameter provides access to the global environment, enabling the function to maintain state across different parts of the AST and resolve identifiers correctly.
- **Error Handling**: During the validation process, the function may encounter syntax or semantic errors. These errors are typically reported back to the user via error messages or exceptions, ensuring they are aware of issues in their code.

By leveraging these interactions, the `check` function effectively ensures that the entire quantum program is valid before proceeding to further compilation phases.