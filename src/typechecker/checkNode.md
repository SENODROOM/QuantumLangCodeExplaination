# `checkNode` Function

## Purpose
The `checkNode` function is an essential part of the Quantum Language compiler's Type Checker module. It validates and verifies the data types of expressions and declarations throughout the compilation process. This ensures that all operations adhere to type safety rules, preventing runtime errors due to incorrect data type usage.

## Parameters
- **node**: A pointer to the current AST node being processed. This can be any type of node such as literals, identifiers, variable declarations, function declarations, or blocks of statements.
- **env**: A shared pointer to the current type environment (`TypeEnv`). The type environment keeps track of the types of variables and functions in the current scope.

## Return Value
- The function returns a string representing the inferred or explicitly specified type of the given AST node. For example, it might return `"float"`, `"string"`, `"bool"`, `"fn"`, etc. If the type cannot be determined, it returns `"any"`.

## Edge Cases
- **Null Node**: If the input `node` is `nullptr`, the function returns `"void"`. This handles cases where there is no valid node to process.
- **Unknown Types**: When encountering an identifier whose type has not been defined yet, the function will return `"any"`. This allows for flexibility in handling undeclared variables until their types are resolved later.
- **Implicit Type Conversion**: The function does not perform implicit type conversions. If a binary expression involves operands of different types and no explicit conversion is provided, a static type warning is issued.

## Interactions with Other Components
- **Type Environment (`TypeEnv`)**: The `checkNode` function interacts closely with the type environment to resolve existing variable types and define new ones. It uses the `resolve` method to find the type of an identifier and the `define` method to add new variables or functions to the environment.
- **AST Nodes**: The function processes various types of AST nodes:
  - **NumberLiteral**: Returns `"float"` since numbers are treated as floating-point values.
  - **StringLiteral**: Returns `"string"` for literal strings.
  - **BoolLiteral**: Returns `"bool"` for boolean literals.
  - **Identifier**: Resolves the type of the identifier using the environment.
  - **VarDecl**: Checks the initializer's type against the user-provided type hint (if available). Defines the variable in the environment with its inferred or specified type.
  - **FunctionDecl**: Sets up a sub-environment for the function's parameters and body, then recursively checks the body. Defines the function itself in the parent environment with a simplified type `"fn"`.
  - **BlockStmt**: Creates a sub-environment for each statement within the block and recursively checks them. Returns `"void"` after processing all statements.

## Detailed Explanation
1. **Null Check**:
   ```cpp
   if (!node) return "void";
   ```
   If the input `node` is `nullptr`, the function immediately returns `"void"`. This prevents further processing on invalid nodes.

2. **Literal Types**:
   ```cpp
   if (node->is<NumberLiteral>()) return "float";
   if (node->is<StringLiteral>()) return "string";
   if (node->is<BoolLiteral>()) return "bool";
   ```
   These conditions handle literal nodes directly. Number literals are always considered as floats, string literals as strings, and boolean literals as bools.

3. **Identifiers**:
   ```cpp
   if (node->is<Identifier>()) {
       return env->resolve(node->as<Identifier>().name);
   }
   ```
   For identifiers, the function looks up their type in the current environment using the `resolve` method. If the identifier is not found, it returns `"any"`.

4. **Variable Declarations**:
   ```cpp
   if (node->is<VarDecl>()) {
       auto& vd = node->as<VarDecl>();
       std::string initType = "any";
       if (vd.initializer) initType = checkNode(vd.initializer, env);
       
       std::string declaredType = vd.typeHint.empty() ? initType : vd.typeHint;
       
       // Basic type check
       if (!vd.typeHint.empty() && vd.typeHint != "any" && initType != "any" && vd.typeHint != initType) {
           std::cerr << Colors::YELLOW << "[StaticTypeWarning] " << Colors::RESET 
                     << "Type mismatch for '" << vd.name << "'. Found " << initType 
                     << " but expected " << vd.typeHint << " (line " << node->line << ")\n";
       }
       
       env->define(vd.name, declaredType);
       return "void";
   }
   ```
   Variable declarations involve checking the type of the initializer (if present) and comparing it with the user-provided type hint. If there is a mismatch, a static type warning is issued. Regardless of whether there is a type hint, the variable is defined in the environment with its type.

5. **Function Declarations**:
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
       env->define(fd