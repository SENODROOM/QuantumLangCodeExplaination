# QuantumLanguage Compiler - TypeChecker.h

## Overview

The `include/TypeChecker.h` header file is an essential part of the QuantumLanguage compiler, responsible for performing static type checking on the Abstract Syntax Tree (AST). This process ensures that the code adheres to the defined types before it is executed, preventing runtime errors due to type mismatches. The `TypeChecker` class manages the type environment during the compilation process, making it possible to track variable types throughout the program.

## Role in Compiler Pipeline

The `TypeChecker.h` file operates early in the compiler pipeline, after the AST has been generated but before code generation or optimization. Its primary function is to validate the types used in the program, ensuring consistency and correctness. If any type errors are detected, the compiler will raise appropriate error messages, halting the compilation process.

## Key Design Decisions and Why

1. **Static Type Checking**: By implementing static type checking, the compiler can catch type-related errors at compile time rather than at runtime. This leads to more robust programs with fewer bugs.

2. **Separate Compilation Units**: The `TypeChecker` class maintains a separate type environment (`TypeEnv`) for each compilation unit. This allows for modular type checking and helps prevent conflicts between different parts of the program.

3. **Inheritance-based Type Environment**: The type environment uses inheritance to manage scoping rules. Variables declared in a local scope shadow those in a parent scope, which simplifies the resolution of variable types.

4. **Exception Handling**: The `TypeChecker` class throws custom exceptions (`StaticTypeError`) when type errors are encountered. These exceptions carry additional information such as the line number where the error occurred, making debugging easier.

## Major Classes/Functions Overview

### Class: `StaticTypeError`

- **Purpose**: Represents a static type error encountered during the compilation process.
- **Attributes**:
  - `line`: The line number where the error occurred.
- **Constructor**:
  - `StaticTypeError(const std::string &msg, int l)`: Initializes the error message and line number.

### Struct: `TypeEnv`

- **Purpose**: Manages the type environment for variables in the program.
- **Attributes**:
  - `vars`: A map storing variable names and their corresponding types.
  - `parent`: A shared pointer to the parent type environment, enabling scoping.
- **Methods**:
  - `define(const std::string& name, const std::string& type)`: Defines a new variable in the current scope.
  - `resolve(const std::string& name)`: Resolves the type of a variable by searching the current and parent scopes.

### Class: `TypeChecker`

- **Purpose**: Performs static type checking on the AST.
- **Attributes**:
  - `globalEnv`: A shared pointer to the global type environment.
- **Methods**:
  - `check(const std::vector<ASTNodePtr>& nodes)`: Checks all nodes in a vector of AST nodes.
  - `check(const ASTNodePtr& node)`: Checks a single AST node.
  - `checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)`: Recursively checks a node within a given type environment.

## Tradeoffs

1. **Compile-Time Overhead**: While static type checking provides benefits, it does introduce some overhead during the compilation process. The compiler must traverse the entire AST multiple times to ensure type consistency.

2. **Complexity**: Implementing a comprehensive type checker requires significant complexity, especially when dealing with advanced features like polymorphism and generics. However, this complexity is mitigated by careful design and extensive testing.

3. **Flexibility**: Static type checking can be restrictive, particularly in dynamically typed languages. QuantumLanguage's approach aims to balance strictness with flexibility through its modular and inheritable type environments.

By understanding these components and considerations, developers can better appreciate the importance of static type checking in the QuantumLanguage compiler and how it contributes to overall code quality and reliability.