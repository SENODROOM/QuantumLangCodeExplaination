# `checkNode` Function

## Purpose
The `checkNode` function is a key component within the Quantum Language compiler's Type Checker module. Its primary role is to determine and validate the data types of expressions and declarations encountered during the compilation process. This ensures that operations are performed on compatible data types, thereby preventing runtime errors related to type mismatches.

## Parameters
- **`node`**: A pointer to the current AST node being processed. The function checks the type of this node based on its structure and content.
- **`env`**: A shared pointer to the current type environment (`TypeEnv`). This environment keeps track of variable and function declarations along with their respective types, facilitating type resolution and validation.

## Return Value
- The function returns a string representing the inferred or explicitly specified type of the given AST node. For example:
  - `"float"` for number literals.
  - `"string"` for string literals.
  - `"bool"` for boolean literals.
  - Variable names resolve to their defined types.
  - Functions are identified as `"fn"`.

## How It Works
1. **Null Check**: If the input `node` is null, the function immediately returns `"void"` indicating an empty or undefined state.
2. **Literal Types**:
   - For number literals, it returns `"float"`.
   - For string literals, it returns `"string"`.
   - For boolean literals, it returns `"bool"`.
3. **Variable Declarations**:
   - When encountering a variable declaration (`VarDecl`), the function first determines the type of the initializer using a recursive call to `checkNode`.
   - It then resolves any user-provided type hint (`typeHint`) against the inferred type of the initializer. If there is a mismatch and the type hint is not `"any"`, it emits a static type warning.
   - The function updates the current type environment (`env`) with the name and resolved type of the variable.
4. **Function Declarations**:
   - For function declarations (`FunctionDecl`), the function sets up a new local type environment (`subEnv`) to handle the parameter types.
   - It iterates over each parameter, inferring or resolving the type based on the provided type hints or defaults to `"any"`.
   - After processing the body of the function, it updates the global type environment with the function's name and a simplified type identifier `"fn"`.
5. **Block Statements**:
   - In the case of a block statement (`BlockStmt`), the function creates a new local type environment (`subEnv`) to isolate the scope of variables and functions within the block.
   - It recursively processes each statement within the block, updating the local environment accordingly.
6. **Binary Expressions**:
   - For binary expressions (`BinaryExpr`), the function would typically infer the type based on the types of the operands and the operation being performed. However, the provided code snippet is incomplete and lacks details on how this functionality is implemented.

## Edge Cases
- **Empty Type Hints**: If a variable or function has an empty type hint, the function assumes the type is `"any"`. This allows flexibility in type specification but may lead to less precise error messages.
- **Null Initializers**: Variables without initializers are treated as having a type of `"any"`, which can result in more lenient type checking but also increases the risk of runtime errors due to implicit type conversions.

## Interactions with Other Components
- **AST Traversal**: The `checkNode` function is called recursively to traverse the entire Abstract Syntax Tree (AST). Each node type handled by the function corresponds to different parts of the language syntax such as literals, variables, functions, and blocks.
- **Error Handling**: Upon detecting a type mismatch, the function outputs a static type warning message. These warnings help developers identify potential issues early in the development cycle, improving code quality and maintainability.
- **Type Environment Management**: The function manages the type environment by defining new variables and functions and resolving existing identifiers. This interaction with the environment is essential for maintaining accurate type information throughout the compilation process.

In summary, the `checkNode` function plays a pivotal role in ensuring type safety within the Quantum Language compiler. It handles various node types, performs type inference and validation, and interacts with the type environment to manage and propagate type information effectively.