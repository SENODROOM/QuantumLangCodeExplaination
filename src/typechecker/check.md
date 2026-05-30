# `check` Function

## Overview

The `check` function is an essential part of the Quantum Language compiler's type-checking phase. It ensures that the source code complies with the language's syntactic and semantic rules. This function operates recursively to traverse the abstract syntax tree (AST) of the source code.

## Parameters

- `node`: A pointer to the current AST node being checked.
- `globalEnv`: A reference to the global environment containing information about types, variables, and functions defined in the program.

## Return Value

This function does not return any value; its primary role is to enforce type safety and catch errors during the compilation process.

## Edge Cases

1. **Null Node**: If the `node` parameter is `nullptr`, the function should handle this case gracefully without causing runtime errors.
2. **Non-Block Statement Nodes**: When encountering nodes that are not block statements (`BlockStmt`), the function calls `checkNode` to perform type checking on individual nodes.

## Interactions with Other Components

- **Type Checker**: The `check` function interacts directly with the type checker to validate expressions, declarations, and function calls within the AST.
- **Global Environment**: It utilizes the `globalEnv` to look up predefined types, variables, and functions, ensuring consistency across the entire program.
- **Recursive Traversal**: By calling itself recursively, the function can handle nested structures in the AST, such as blocks within blocks or loops containing conditional statements.

## Implementation Details

Here's a breakdown of how the `check` function works:

```cpp
void check(ASTNode* node, GlobalEnvironment& globalEnv) {
    // Check if the current node is a block statement
    if (node && node->is<BlockStmt>()) {
        // Recursively check each statement inside the block
        BlockStmt* block = node->as<BlockStmt>();
        for (auto stmt : block->statements) {
            check(stmt, globalEnv);
        }
    } else {
        // Otherwise, check the current node individually
        checkNode(node, globalEnv);
    }
}
```

### Key Points

- **Block Statements**: If the current node is a block statement, the function iterates through each statement contained within the block and recursively calls `check` on them. This allows for comprehensive type checking of compound statements.
- **Individual Nodes**: For all other types of nodes, the function invokes `checkNode`, which performs more granular type checking based on the specific nature of the node.

By leveraging recursion and interacting with the global environment, the `check` function efficiently traverses the AST and ensures that all elements adhere to the Quantum Language's type system, thereby preventing runtime errors and improving the reliability of the compiled code.