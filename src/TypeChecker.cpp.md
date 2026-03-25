# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` is a critical component of the Quantum Language compiler, tasked with performing static type checking on the Abstract Syntax Tree (AST). This process ensures that all expressions and statements are consistent with their declared types, thus preventing runtime errors caused by type mismatches.

## Role in the Compiler Pipeline

The `TypeChecker` plays a pivotal role in the compilation process, following the parsing stage. After the AST is constructed, it undergoes static analysis to verify type correctness. The checker traverses each node in the AST, applying type rules and constraints. If any inconsistencies are found, warnings or errors are generated, guiding developers towards correcting their code before execution.

## Key Design Decisions and Trade-offs

### Why Static Type Checking?

- **Preventing Runtime Errors**: By identifying type mismatches at compile time, potential runtime errors are avoided, enhancing program reliability.
- **Code Readability and Maintenance**: Explicit type declarations make the code more understandable and easier to maintain.
- **Efficiency**: Early detection of type issues reduces debugging time and improves overall development efficiency.

### Design Choices

1. **Built-in Types**: The `TypeChecker` initializes a global environment with predefined types such as `print`, `input`, `len`, `sha256`, and `aes128`. This simplifies handling these common functions without requiring explicit type definitions.
   
2. **Scope Management**: Each function and block statement has its own scope, managed through nested environments (`TypeEnv`). This allows for local variable type resolution and prevents name clashes between different scopes.

3. **Type Inference**: For variables without explicit type hints, the checker infers the type based on the initializer expression. This enhances flexibility while maintaining strong typing.

4. **Type Hints**: Developers can provide type hints for variables and function parameters using the `typeHint` field. These hints are used during the type checking process to enforce consistency.

### Trade-offs

- **Flexibility vs. Strictness**: Allowing type inference provides some level of flexibility, but it may lead to unexpected behavior if not handled carefully. Enforcing strict type matching offers better predictability but restricts code flexibility.
  
- **Complexity**: Managing nested environments and handling various type operations adds complexity to the implementation. However, this complexity is necessary for robust type checking.

## Major Classes/Functions Overview

### Class: `TypeChecker`

- **Constructor**: Initializes the global environment with predefined types.
- **Method: `check(const std::vector<ASTNodePtr>& nodes)`**
  - Iterates over a vector of AST nodes and checks each one.
- **Method: `check(const ASTNodePtr& node)`**
  - Checks an individual AST node, handling blocks recursively.
- **Method: `checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)`**
  - Performs the actual type checking for a given node within a specific environment.
  - Handles number literals, string literals, boolean literals, identifiers, variable declarations, function declarations, and binary expressions.

### Class: `TypeEnv`

- Represents the type environment, which tracks variable names and their associated types.
- Provides methods to define new variables, resolve existing ones, and manage nested scopes.

## Conclusion

The `TypeChecker.cpp` module is essential for ensuring the type safety of the Quantum Language programs. Through careful design and implementation, it balances flexibility with strictness, providing valuable feedback during the development phase. The use of nested environments and type inference mechanisms makes it a powerful tool in the compiler's arsenal, ultimately contributing to the creation of reliable and efficient quantum applications.