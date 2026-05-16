# QuantumLanguage Compiler - TypeChecker.h

## Overview

The `include/TypeChecker.h` header file is a crucial component of the QuantumLanguage compiler, dedicated to type checking during the compilation process. This ensures that the code adheres to its intended data types, preventing runtime errors due to type mismatches and enhancing overall code reliability and maintainability.

## Role in Compiler Pipeline

### Key Phases and Responsibilities:

1. **Parsing**: Converts source code into an Abstract Syntax Tree (AST).
2. **Type Checking**: Ensures all expressions and declarations conform to their specified types.
3. **Code Generation**: Translates the AST into executable bytecode or machine code.

### Why Type Checking?

- **Preventing Runtime Errors**: By identifying type mismatches early, type checking helps prevent runtime errors that could be difficult to debug.
- **Enhancing Code Reliability**: Ensuring type safety improves the robustness of the compiled code.
- **Maintaining Developer Productivity**: Early detection of type issues saves time and effort in debugging later on.

## Major Classes and Functions Overview

### Class Definitions:

#### `StaticTypeError`
- Inherits from `std::runtime_error`.
- Represents a static type error, which occurs during compile-time rather than runtime.
- Contains an additional member variable `line` to indicate the line number where the error occurred.

#### `TypeEnv`
- A structure representing the type environment used during type checking.
- Manages variables and their associated types using a map (`std::map<std::string, std::string>`).
- Supports hierarchical environments through a shared pointer to a parent `TypeEnv`.

#### `TypeChecker`
- A class responsible for performing type checking on the AST.
- Initializes with a global type environment (`globalEnv`).
- Provides methods to check entire lists of AST nodes (`check`) and individual nodes (`checkNode`).

### Function Prototypes:

- `void check(const std::vector<ASTNodePtr>& nodes)`: Checks a list of AST nodes.
- `void check(const ASTNodePtr& node)`: Checks a single AST node.
- `std::string checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)`: Recursively checks a node within a given type environment and returns the inferred type.

## Tradeoffs

### Benefits:

- **Early Error Detection**: Type checking catches errors at compile-time, reducing the likelihood of runtime bugs.
- **Improved Code Quality**: Ensures that the codebase is free from type-related issues, leading to higher-quality software.
- **Facilitates Debugging**: Easier to identify and fix type-related problems since they occur before the code runs.

### Considerations:

- **Complexity**: Implementing a comprehensive type checker can add significant complexity to the compiler.
- **Performance Overhead**: Type checking may introduce some performance overhead, although this is often mitigated by optimizations.
- **Language Design**: The effectiveness of type checking depends on the language's design and features. Some languages may have more complex type systems requiring more sophisticated type checkers.

In summary, the `TypeChecker.h` header file plays a vital role in the QuantumLanguage compiler by ensuring type safety during the compilation process. It leverages custom exception handling, hierarchical type environments, and recursive type checking to validate the AST and catch potential errors early. While this adds complexity and may impact performance, it significantly enhances the reliability and quality of the generated code.