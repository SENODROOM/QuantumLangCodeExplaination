# `checkNode` Function

## Purpose
The `checkNode` function is an essential part of the Quantum Language compiler's Type Checker module. It is responsible for verifying and validating the data types of expressions and declarations encountered during the compilation process. This ensures that the code adheres to the specified type constraints, preventing runtime errors due to type mismatches.

## Parameters
- **node**: A pointer to the current AST (Abstract Syntax Tree) node being checked. The function processes different types of nodes such as literals, identifiers, variable declarations, function declarations, and blocks.
- **env**: A shared pointer to the current type environment (`TypeEnv`). This environment keeps track of the variables and their associated types within the scope of the current node.

## Return Value
- The function returns a string representing the inferred or explicitly stated type of the given node. For example:
  - `"float"` if the node is a number literal.
  - `"string"` if the node is a string literal.
  - `"bool"` if the node is a boolean literal.
  - `"any"` if the node's type cannot be determined or if there is no explicit type hint provided.

## Edge Cases
1. **Null Node**: If the input `node` is `nullptr`, the function returns `"void"`.
2. **Implicit Types**: When a variable declaration lacks an explicit type hint, the function infers its type based on the initializer expression.
3. **Type Mismatch Warnings**: If an explicit type hint is provided and does not match the inferred type, the function emits a static type warning indicating the discrepancy.

## Interactions with Other Components
- **AST Traversal**: The `checkNode` function is invoked recursively to traverse the entire AST. Each node is processed according to its type, and the function updates the type environment accordingly.
- **Error Handling**: During type checking, the function uses `std::cerr` to output warnings about potential type mismatches. These warnings help developers identify and correct issues before the code is executed.
- **Scope Management**: The function creates new type environments for nested blocks and functions using `std::make_shared<TypeEnv>(env)`. This allows for proper management of variable scopes and their respective types.

## Detailed Explanation
### Number Literal
If the node is a `NumberLiteral`, the function returns `"float"` because quantum operations often involve floating-point numbers.

### String Literal
For a `StringLiteral`, the function returns `"string"` since strings represent textual data.

### Boolean Literal
Boolean literals are handled similarly to string literals, returning `"bool"`.

### Identifier
When encountering an `Identifier`, the function resolves its type using the provided environment (`env->resolve(node->as<Identifier>().name)`). This step is crucial for ensuring that all references to variables have valid types.

### Variable Declaration
If the node is a `VarDecl`, the function performs several checks:
1. **Initializer Type Check**: It determines the type of the initializer expression by calling `checkNode(vd.initializer, env)`.
2. **Type Hint Validation**: If an explicit type hint (`vd.typeHint`) is provided, it compares it with the initializer type. If they do not match, a static type warning is emitted.
3. **Environment Update**: The function defines the variable in the current environment with either the inferred type or the explicitly stated type hint (`env->define(vd.name, declaredType)`).

### Function Declaration
For a `FunctionDecl`, the function handles:
1. **Parameter Type Assignment**: It assigns types to each parameter based on the provided `paramTypes` vector. If no type is specified, it defaults to `"any"`.
2. **Nested Environment Creation**: It creates a new nested environment (`subEnv`) to manage the types of parameters within the function body.
3. **Body Type Checking**: It recursively checks the type of the function body using the nested environment.
4. **Return Type Assignment**: If an explicit return type (`fd.returnType`) is provided, it assigns this type to the function in the parent environment. Otherwise, it defaults to `"any"`.

### Block Statement
Blocks are processed by creating a new nested environment (`subEnv`) and iterating through each statement within the block. Each statement is checked recursively, allowing for local scope resolution and type validation.

## Example Usage
Here’s how you might use the `checkNode` function in a hypothetical scenario:

```cpp
// Create an empty type environment
auto env = std::make_shared<TypeEnv>();

// Parse some source code into an AST
auto ast = parseSourceCode("let x = 5;");

// Check the type of each node in the AST
for (auto& node : ast->nodes) {
    std::cout << "Node type: " << checkNode(node, env) << "\n";
}
```

In this example, the `checkNode` function would infer the type of the identifier `x` as `"float"` since the initializer `5` is a number literal.

## Conclusion
The `checkNode` function plays a vital role in ensuring the type safety of quantum programs. By recursively traversing the AST and updating the type environment, it helps catch potential errors early in the compilation process, making debugging more straightforward.