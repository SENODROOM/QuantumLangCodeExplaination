# `checkNode` Function

## Purpose
The `checkNode` function is an integral component of the Quantum Language compiler's Type Checker module. Its primary responsibility is to validate and verify the data types of expressions and declarations as they are processed during the compilation phase. This step is crucial for ensuring that the code adheres to its intended data type requirements and prevents runtime errors due to type mismatches.

## Parameters
- **`node`**: A pointer to the current AST (Abstract Syntax Tree) node being checked. This can represent various elements such as literals, identifiers, variable declarations, function declarations, and blocks of statements.
- **`env`**: A shared pointer to the current type environment (`TypeEnv`). The type environment maintains information about the variables and functions in scope, including their types. This allows `checkNode` to resolve and update types based on the context in which nodes appear.

## Return Value
- The function returns a string representing the inferred or explicitly declared type of the given node. For example:
  - `"float"` for numeric literals.
  - `"string"` for string literals.
  - `"bool"` for boolean literals.
  - `"any"` for undeclared or dynamically typed nodes.
  
## How It Works
1. **Null Check**: If the input `node` is null, the function immediately returns `"void"`.
2. **Literal Types**:
   - If the node is a `NumberLiteral`, it returns `"float"`.
   - If the node is a `StringLiteral`, it returns `"string"`.
   - If the node is a `BoolLiteral`, it returns `"bool"`.
3. **Identifiers**:
   - If the node is an `Identifier`, the function attempts to resolve its type using the provided `env`. If the identifier is not found in the environment, it defaults to `"any"`.
4. **Variable Declarations**:
   - If the node is a `VarDecl`, it checks the initializer to determine the type of the variable.
   - If a `typeHint` is provided in the declaration, it uses that hint to declare the variable’s type; otherwise, it uses the type determined from the initializer.
   - A basic type check is performed to ensure that the `typeHint` matches the actual type of the initializer. If there is a mismatch, a warning message is printed indicating the line number where the error occurred.
   - The variable is then defined in the environment with its resolved type.
5. **Function Declarations**:
   - If the node is a `FunctionDecl`, it sets up a new sub-environment (`subEnv`) to handle the types within the function scope.
   - Each parameter in the function is assigned a type, either from the `paramTypes` array or defaulted to `"any"` if no explicit type is provided.
   - The body of the function is recursively checked using the `subEnv`.
   - The function itself is defined in the main environment with a simplified type `"fn"`.
6. **Blocks of Statements**:
   - If the node is a `BlockStmt`, a new sub-environment is created to encapsulate the local scope.
   - Each statement within the block is checked sequentially using the `subEnv`.

## Edge Cases
- **Null Nodes**: The function handles null nodes gracefully by returning `"void"`.
- **Undeclared Identifiers**: If an identifier is used without being declared, it defaults to `"any"`.
- **Empty Type Hints**: When a variable or function declaration lacks a type hint, the function infers the type from the initializer or assigns it as `"any"`.

## Interactions with Other Components
- **Type Environment (`TypeEnv`)**: `checkNode` interacts closely with the type environment to resolve existing types and define new ones. This ensures that all types are correctly tracked throughout the compilation process.
- **AST Traversal**: As part of the broader compilation pipeline, `checkNode` is invoked recursively to traverse the entire AST. This allows it to perform type checking at every level of the program structure.
- **Error Handling**: During type checking, warnings are generated for type mismatches, providing feedback to the developer to improve code quality.

By performing these comprehensive type checks, `checkNode` helps maintain the integrity and correctness of the quantum codebase, ensuring efficient and accurate execution of programs.