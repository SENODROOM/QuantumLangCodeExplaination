# QuantumLanguage Compiler - TypeChecker.h

## Overview

The `include/TypeChecker.h` header file is a crucial component of the QuantumLanguage compiler, focusing on the static type checking process. This module ensures that the code adheres to its declared types before compilation proceeds, preventing runtime errors due to type mismatches.

## Role in Compiler Pipeline

The `TypeChecker.h` plays a pivotal role in the compiler's pipeline, specifically during the semantic analysis phase. After parsing and generating Abstract Syntax Trees (ASTs), the TypeChecker verifies the types of variables, expressions, and function calls. It constructs a type environment (`TypeEnv`) which tracks variable declarations and their associated types throughout the program. If any type inconsistencies are detected, it raises a `StaticTypeError`, halting the compilation process and providing a clear error message along with the line number where the issue occurred.

## Key Design Decisions and Why

1. **Use of Exceptions**: By throwing `StaticTypeError`, the TypeChecker enforces strict adherence to type rules. This approach simplifies error handling and makes the type checking process more robust and user-friendly.

2. **Hierarchical Type Environment**: The `TypeEnv` struct supports a hierarchical structure through its `parent` pointer, allowing for nested scopes. This design facilitates the resolution of variable types even when they are defined in different parts of the program, enhancing the flexibility and scalability of the compiler.

3. **Shared Pointer for Environment Management**: Utilizing `std::shared_ptr` for managing the type environment ensures automatic memory cleanup and prevents memory leaks, making the implementation cleaner and more maintainable.

4. **Separation of Concerns**: The `TypeChecker` class encapsulates the logic for type checking, keeping the main compiler flow clean and organized. This separation allows for easier testing and maintenance of the type checking mechanism.

## Major Classes/Functions Overview

### Class: `StaticTypeError`
- **Inheritance**: Inherits from `std::runtime_error`.
- **Purpose**: Custom exception class used to indicate static type errors.
- **Attributes**:
  - `int line`: Stores the line number where the error occurred.
- **Constructor**: Initializes the exception with a message and a line number.

### Struct: `TypeEnv`
- **Purpose**: Represents the type environment, tracking variable names and their types.
- **Attributes**:
  - `std::map<std::string, std::string> vars`: Maps variable names to their types.
  - `std::shared_ptr<TypeEnv> parent`: Points to the parent scope's type environment.
- **Methods**:
  - `void define(const std::string& name, const std::string& type)`: Defines a new variable in the current scope.
  - `std::string resolve(const std::string& name)`: Resolves the type of a variable by searching the current and parent scopes.

### Class: `TypeChecker`
- **Purpose**: Manages the type checking process for the entire program.
- **Attributes**:
  - `std::shared_ptr<TypeEnv> globalEnv`: Holds the global type environment.
- **Methods**:
  - `TypeChecker()`: Constructor initializes the global type environment.
  - `void check(const std::vector<ASTNodePtr>& nodes)`: Checks the type of each node in a vector of AST nodes.
  - `void check(const ASTNodePtr& node)`: Checks the type of a single AST node.
  - `std::string checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)`: Recursively checks the type of a node within a given type environment and returns the resolved type.

## Tradeoffs

- **Performance vs. Robustness**: While the use of exceptions can make the type checking process more robust, it may introduce performance overhead compared to other error handling mechanisms. However, this tradeoff is justified by the need to catch and report type-related errors early in the compilation process.

- **Complexity vs. Flexibility**: The hierarchical type environment adds complexity to the implementation but significantly enhances the flexibility of the compiler, allowing it to handle complex scoping scenarios accurately.

- **Memory Usage**: Using `std::shared_ptr` for managing the type environment increases memory usage due to reference counting. However, this is generally outweighed by the benefits of automatic memory management and avoiding memory leaks.

Overall, the `TypeChecker.h` provides a comprehensive solution for static type checking in the QuantumLanguage compiler, balancing robustness, flexibility, and performance considerations.