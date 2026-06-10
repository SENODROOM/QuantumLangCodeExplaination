# `checkNode` Function

## Purpose
The `checkNode` function is an essential part of the Quantum Language compiler's Type Checker module. Its main objective is to ensure that all expressions and declarations within the code adhere to the correct data types, thereby maintaining type safety throughout the compilation process.

## Parameters
- **`node`**: A pointer to the current AST node being processed. This can be any type of node such as a number literal, string literal, boolean literal, identifier, variable declaration, function declaration, block statement, or binary expression.
- **`env`**: A shared pointer to the current type environment (`TypeEnv`). The type environment keeps track of the types of variables and functions in the current scope.

## Return Value
- Returns a string representing the inferred or explicitly specified type of the given AST node. For example, it might return `"float"` for a number literal, `"string"` for a string literal, `"bool"` for a boolean literal, or `"fn"` for a function declaration.

## How It Works
1. **Base Case**: If the input `node` is `nullptr`, the function returns `"void"`.
2. **Number Literals**: If the node is a `NumberLiteral`, the function returns `"float"`. This is because quantum computations often involve floating-point numbers.
3. **String Literals**: If the node is a `StringLiteral`, the function returns `"string"`. String literals are straightforward in terms of their type.
4. **Boolean Literals**: If the node is a `BoolLiteral`, the function returns `"bool"`. Boolean values are fundamental in programming.
5. **Identifiers**: If the node is an `Identifier`, the function resolves its type using the provided type environment (`env`) and returns the resolved type.
6. **Variable Declarations**: If the node is a `VarDecl`, the function:
   - Checks the initializer of the variable. If there is an initializer, it recursively calls `checkNode` on the initializer to determine its type.
   - Uses the `typeHint` field of the variable declaration to get the explicitly specified type. If `typeHint` is empty, it defaults to the type determined by the initializer.
   - Performs a basic type check to ensure that the explicitly specified type matches the type determined by the initializer. If they do not match, it emits a static type warning indicating the mismatch.
   - Defines the variable in the current type environment with its resolved or explicitly specified type.
7. **Function Declarations**: If the node is a `FunctionDecl`, the function:
   - Creates a new type environment (`subEnv`) that inherits from the current one (`env`).
   - Iterates over the parameters of the function. For each parameter, it checks the corresponding entry in `paramTypes` to determine the explicitly specified type. If `paramTypes` is empty for a parameter, it defaults to `"any"`.
   - Defines each parameter in the `subEnv` with its explicitly specified or default type.
   - Recursively calls `checkNode` on the body of the function to perform type checking within the function's scope.
   - Uses the `returnType` field of the function declaration to get the explicitly specified return type. If `returnType` is empty, it defaults to `"any"`.
   - Defines the function in the current type environment with the type `"fn"`, indicating that it is a function.
8. **Block Statements**: If the node is a `BlockStmt`, the function:
   - Creates a new type environment (`subEnv`) that inherits from the current one (`env`).
   - Iterates over each statement within the block and recursively calls `checkNode` on each statement.
   - Finally, returns `"void"` after processing all statements in the block.

## Edge Cases
- **Empty Initializers**: When a variable is declared without an initializer, `initType` defaults to `"any"`.
- **Missing Type Hints**: When a variable or function is declared without an explicit type hint, the type is determined by the initializer or body respectively.
- **Type Mismatches**: The function emits a static type warning when the explicitly specified type does not match the type determined by the initializer or body.
- **Nested Scopes**: The function uses nested type environments (`subEnv`) to handle different scopes within the code, ensuring that variable and function types are correctly resolved and checked.

## Interactions With Other Components
- **AST Parser**: The `checkNode` function receives nodes from the AST parser, which constructs the abstract syntax tree based on the source code.
- **Type Environment**: The function interacts with the type environment to resolve and define types, managing the scope and lifetime of variables and functions.
- **Error Reporting**: The function reports errors and warnings through standard error streams, providing feedback to the user about potential issues in the code.
- **Scope Management**: The function handles different scopes within the code by creating and using nested type environments, ensuring that types are correctly resolved and validated at each level.