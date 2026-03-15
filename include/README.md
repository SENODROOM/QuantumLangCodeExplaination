# QuantumLanguage Compiler - TypeChecker.h

## Overview

The `include/TypeChecker.h` header file is a crucial component of the QuantumLanguage compiler, responsible for ensuring that the code adheres to its type system during compilation. This file plays a vital role in the compiler pipeline, acting as a bridge between the abstract syntax tree (AST) generation phase and the code generation phase. It helps catch errors early in the development process, improving overall code quality and maintainability.

### Key Design Decisions and Why

1. **Static Typing with Runtime Error Handling**:
   - The compiler uses static typing to enforce type safety at compile time. However, it also handles runtime type errors gracefully using exceptions (`StaticTypeError`). This dual approach ensures both compile-time checks and robust error handling at runtime.

2. **Hierarchical Type Environment**:
   - A hierarchical type environment (`TypeEnv`) is implemented to manage variable types in nested scopes. This design allows for easy resolution of variable types even when they are defined in different scopes, providing flexibility and scalability in handling complex programs.

3. **Separation of Concerns**:
   - The `TypeChecker` class encapsulates the logic for type checking, making the code modular and easier to understand. By separating type checking into individual methods, such as `check`, `checkNode`, and `define`, the class becomes more manageable and reusable.

## Major Classes/Functions Overview

### 1. `StaticTypeError`
- **Role**: Inherits from `std::runtime_error` and includes additional information about the line number where the error occurred.
- **Why**: Provides a clear and specific way to report type-related errors, facilitating debugging and maintenance.

### 2. `TypeEnv`
- **Role**: Represents a type environment, which maps variable names to their types. It supports hierarchical environments through a shared pointer to a parent environment.
- **Why**: Enables efficient management of variable types across different scopes, supporting nested functions and blocks without losing track of variable definitions.

### 3. `TypeChecker`
- **Role**: Manages the overall type checking process. It initializes a global type environment and provides methods to check entire ASTs or individual nodes.
- **Why**: Centralizes the type checking logic, making it easier to integrate and extend. The separation of concerns allows for focused development and testing of each component.

## Tradeoffs

1. **Performance vs. Flexibility**:
   - Using exceptions for runtime type errors can be slower compared to other error handling mechanisms. However, it provides greater flexibility and clarity in reporting errors, especially in complex scenarios involving multiple nested scopes.

2. **Memory Usage**:
   - Hierarchical type environments require additional memory to store parent pointers. While this adds overhead, it enhances the expressiveness and usability of the type system, particularly in large and complex programs.

3. **Complexity vs. Simplicity**:
   - Encapsulating type checking logic within a single class (`TypeChecker`) simplifies the interface but may increase complexity internally due to the need to manage state and handle different types of AST nodes. Balancing these factors is essential for maintaining a maintainable and scalable codebase.

Overall, the `TypeChecker.h` file is designed to provide a comprehensive and flexible type checking mechanism for the QuantumLanguage compiler, while balancing performance, memory usage, and complexity considerations.