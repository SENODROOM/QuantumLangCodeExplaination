# QuantumLanguage Compiler - TypeChecker.h

## Overview

The `include/TypeChecker.h` header file is an essential part of the QuantumLanguage compiler, specifically designed to handle the type checking process during the compilation phase. The primary role of the type checker is to ensure that all expressions and statements in the source code adhere to the defined type rules, thereby preventing runtime errors due to type mismatches.

## Role in the Compiler Pipeline

1. **Parsing**: Converts the source code into an Abstract Syntax Tree (AST).
2. **Type Checking**: Ensures that the types used in the code are valid according to the language's type system.
3. **Code Generation**: Translates the AST into executable code.

The type checker operates after parsing but before code generation. It traverses the AST, examining each node to validate its type against the expected type based on the language's syntax and semantics. If any type mismatch is detected, it raises a `StaticTypeError`, which includes the error message and the line number where the error occurred.

## Key Design Decisions and Why

### 1. Use of Exception Handling

- **Why**: To provide clear and informative error messages directly at the point of type violation, enhancing debugging and user experience.
- **Implementation**: The `StaticTypeError` class inherits from `std::runtime_error`, making it easy to integrate with existing exception handling mechanisms in C++.

### 2. Hierarchical Type Environment (`TypeEnv`)

- **Why**: To manage variable scoping and inheritance, allowing nested scopes and resolving variables within their respective environments.
- **Implementation**: The `TypeEnv` struct contains a map of variable names to their types and a pointer to a parent environment. This hierarchical structure supports both local and global variable resolution.

### 3. Separate Class for Type Checking (`TypeChecker`)

- **Why**: To encapsulate the type checking logic, making the codebase more modular and easier to maintain.
- **Implementation**: The `TypeChecker` class has methods to check entire programs (`check(const std::vector<ASTNodePtr>& nodes)`) and individual nodes (`check(const ASTNodePtr& node)`). It also provides a method to check a single node with a given type environment (`checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)`).

## Major Classes/Functions Overview

### `StaticTypeError`

- **Purpose**: Represents a static type error encountered during the type checking process.
- **Attributes**:
  - `int line`: Stores the line number where the error occurred.
- **Constructor**: Initializes the error message and line number.

### `TypeEnv`

- **Purpose**: Manages the type environment for variable declarations and lookups.
- **Attributes**:
  - `std::map<std::string, std::string> vars`: A map storing variable names and their types.
  - `std::shared_ptr<TypeEnv> parent`: A pointer to the parent type environment, supporting scope nesting.
- **Methods**:
  - `void define(const std::string& name, const std::string& type)`: Defines a new variable in the current environment.
  - `std::string resolve(const std::string& name)`: Resolves the type of a variable, searching in the current environment and then its parent environments if necessary.

### `TypeChecker`

- **Purpose**: Orchestrates the type checking process for the entire program or individual nodes.
- **Attributes**:
  - `std::shared_ptr<TypeEnv> globalEnv`: The top-level type environment containing global variable definitions.
- **Methods**:
  - `TypeChecker()`: Constructor initializes the global type environment.
  - `void check(const std::vector<ASTNodePtr>& nodes)`: Checks a list of AST nodes, typically representing a complete program.
  - `void check(const ASTNodePtr& node)`: Checks a single AST node.
  - `std::string checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)`: Recursively checks the type of an AST node within a given type environment.

## Tradeoffs

- **Performance vs. Completeness**: While the type checker ensures type safety, its thoroughness can impact performance, especially for large programs. Optimizing the type checking algorithm without compromising correctness is a significant challenge.
- **Complexity vs. Usability**: The use of exception handling and hierarchical type environments adds complexity to the implementation. However, these features enhance usability by providing clear, actionable error messages and support for complex type systems.

Overall, the `TypeChecker.h` file plays a crucial role in ensuring the reliability and correctness of the QuantumLanguage compiler by validating the types of expressions and statements in the source code. Its design decisions balance completeness, performance, and usability, making it a vital component of the compiler's architecture.