# `checkNode` Function

## Purpose
The `checkNode` function is an integral component of the Quantum Language compiler's Type Checker module. Its primary role is to validate and verify the data types of expressions and declarations during the compilation process. By ensuring type safety, this function helps prevent runtime errors related to incorrect data type usage.

## Parameters
- **`node`:** A pointer to the current AST (Abstract Syntax Tree) node being processed. This can represent various elements in the code such as literals, identifiers, variable declarations, function declarations, blocks, or binary expressions.
- **`env`:** A shared pointer to the current `TypeEnv` environment, which holds information about the types of variables and functions in scope.

## Return Value
- The function returns a string representing the data type of the given AST node. For example:
  - `"float"` for number literals,
  - `"string"` for string literals,
  - `"bool"` for boolean literals,
  - The resolved type for identifiers,
  - `"void"` for variable and function declarations, and
  - The result of checking binary expressions.

## Edge Cases
- **Empty Node:** If the input `node` is `nullptr`, the function returns `"void"`.
- **Unresolved Identifier:** If an identifier cannot be resolved in the current environment, the function may throw an error or return an unspecified type.
- **Inconsistent Types:** When a variable declaration includes both a type hint and an initializer, the function checks if these types match. If they do not, it emits a static type warning indicating the mismatch.

## Interactions with Other Components
- **AST Traversal:** `checkNode` is typically invoked recursively to traverse the entire AST. Each node type has its own logic within the function to determine its type.
- **Environment Management:** The function uses the provided `TypeEnv` object (`env`) to manage and resolve types. New environments are created for function bodies and blocks to handle local scoping.
- **Error Reporting:** Static type warnings are reported through standard error output using `std::cerr`. These warnings help developers identify potential issues before runtime execution.

## Implementation Details
Here’s a breakdown of how `checkNode` handles different types of nodes:

1. **Number Literal:**
   ```cpp
   if (node->is<NumberLiteral>()) return "float";
   ```
   Number literals are always treated as floating-point numbers.

2. **String Literal:**
   ```cpp
   if (node->is<StringLiteral>()) return "string";
   ```
   String literals are treated as strings.

3. **Boolean Literal:**
   ```cpp
   if (node->is<BoolLiteral>()) return "bool";
   ```
   Boolean literals are treated as booleans.

4. **Identifier:**
   ```cpp
   if (node->is<Identifier>()) {
       return env->resolve(node->as<Identifier>().name);
   }
   ```
   Identifiers are resolved to their corresponding types in the current environment.

5. **Variable Declaration:**
   ```cpp
   if (node->is<VarDecl>()) {
       auto& vd = node->as<VarDecl>();
       std::string initType = "any";
       if (vd.initializer) initType = checkNode(vd.initializer, env);
       
       std::string declaredType = vd.typeHint.empty() ? initType : vd.typeHint;
       
       if (!vd.typeHint.empty() && vd.typeHint != "any" && initType != "any" && vd.typeHint != initType) {
           std::cerr << Colors::YELLOW << "[StaticTypeWarning] " << Colors::RESET 
                     << "Type mismatch for '" << vd.name << "'. Found " << initType 
                     << " but expected " << vd.typeHint << " (line " << node->line << ")\n";
       }
       
       env->define(vd.name, declaredType);
       return "void";
   }
   ```
   Variable declarations are checked against their initializers. If a type hint is provided, it must match the inferred type of the initializer. The resolved type is then defined in the environment.

6. **Function Declaration:**
   ```cpp
   if (node->is<FunctionDecl>()) {
       auto& fd = node->as<FunctionDecl>();
       auto subEnv = std::make_shared<TypeEnv>(env);
       for (size_t i = 0; i < fd.params.size(); ++i) {
           std::string pType = "any";
           if (i < fd.paramTypes.size() && !fd.paramTypes[i].empty()) pType = fd.paramTypes[i];
           subEnv->define(fd.params[i], pType);
       }
       checkNode(fd.body, subEnv);
       
       std::string retType = fd.returnType.empty() ? "any" : fd.returnType;
       env->define(fd.name, "fn");
       return "void";
   }
   ```
   Function declarations define new scopes with their own environment (`subEnv`). Parameters are added to this sub-environment, and the body of the function is checked within this context. The function itself is marked as having a type of `"fn"` in the outer environment.

7. **Block Statement:**
   ```cpp
   if (node->is<BlockStmt>()) {
       auto& block = node->as<BlockStmt>();
       auto subEnv = std::make_shared<TypeEnv>(env);
       for (auto& stmt : block.statements) {
           checkNode(stmt, subEnv);
       }
       return "void";
   }
   ```
   Block statements create new scopes, and each statement within the block is checked independently in the new environment.

8. **Binary Expression:**
   ```cpp
   if (node->is<BinaryExpr>()) {
       auto