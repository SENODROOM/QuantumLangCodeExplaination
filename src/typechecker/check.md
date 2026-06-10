# `check` Function

## Overview

The `check` function is a crucial component of the Quantum Language compiler's type-checking phase. Its primary role is to ensure that the source code adheres to both syntactic and semantic rules defined by the language. The function operates recursively to traverse the Abstract Syntax Tree (AST), applying various checks at each node to validate its correctness.

### Why It Works This Way

The recursive nature of the `check` function allows it to handle nested structures within the AST effectively. By breaking down the problem into smaller subproblems, the function can apply specific checks to different types of nodes without losing context about their parent or sibling nodes. This approach ensures comprehensive validation across all parts of the source code.

## Parameters

- **`node`**: A pointer to the current node in the AST being processed. This parameter represents the element being checked.
- **`globalEnv`**: A reference to the global environment object, which contains information about the scope, variables, and functions available during the compilation process.

## Return Value

The `check` function does not explicitly return a value; instead, it performs in-place validation. If any errors are detected during the checking process, they are reported through error messages or exceptions, depending on the implementation details of the compiler.

## Edge Cases

1. **Empty Nodes**: If the `node` pointer is `nullptr`, the function simply returns without performing any checks. This handles cases where there might be optional or empty nodes in the AST.
2. **Non-Block Statements**: When encountering nodes that are not instances of `BlockStmt`, the function calls `checkNode` directly. This method is responsible for handling individual statements and expressions, ensuring they meet the necessary criteria.
3. **Nested Blocks**: For nodes that represent blocks (`BlockStmt`), the function recursively processes each statement within the block. This ensures that even deeply nested structures are validated correctly.

## Interactions with Other Components

The `check` function interacts closely with several other components of the Quantum Language compiler:

1. **Scope Management**: During the checking process, the function updates and manages the scope based on the declarations encountered. This helps in resolving variable names and ensuring that they are used within their valid scope.
2. **Type Inference**: The function uses type inference to determine the types of expressions and variables. This involves analyzing the structure and content of nodes to deduce their types accurately.
3. **Error Reporting**: If any issues are found during the checking, such as undefined variables, type mismatches, or syntactic errors, the function reports these errors using appropriate mechanisms provided by the compiler framework.
4. **Semantic Analysis**: Beyond basic type checking, the `check` function may also perform semantic analysis to ensure that the code follows logical and structural constraints imposed by the language.

Overall, the `check` function plays a vital role in maintaining the integrity and correctness of the compiled quantum programs by rigorously validating their structure and semantics against the language specifications.