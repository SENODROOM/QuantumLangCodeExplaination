# QuantumLanguage Compiler - TypeChecker.h

## Overview

The `include/TypeChecker.h` header file plays a pivotal role in the QuantumLanguage compiler's pipeline, ensuring that all code adheres to its specified type system during the compilation process. This file is essential for detecting static type errors before runtime execution, thereby enhancing the robustness and reliability of the compiled programs.

### Key Design Decisions and Why

1. **Static Error Detection**: The primary reason for implementing a type checker is to catch type-related errors at compile time rather than runtime. This prevents potential crashes or unexpected behavior due to incorrect type usage.

2. **Hierarchical Type Environment**: To manage variable scoping effectively, the type checker uses a hierarchical type environment (`TypeEnv`). Each scope has its own environment that inherits from its parent, allowing for nested scopes with different variable bindings.

3. **Exception Handling**: The `StaticTypeError` class is designed as an exception to handle cases where static type checking fails. It includes the error message and the line number where the error occurred, making it easier to debug issues.

4. **Flexibility in Type System**: The type system is designed to be flexible, allowing for both explicit and implicit type declarations. This accommodates different programming styles and reduces boilerplate code.

## Major Classes/Functions Overview

### 1. `StaticTypeError`
- **Role**: Represents a static type error encountered during compilation.
- **Why**: Provides a structured way to handle and report type-related errors.
- **Overview**: Inherits from `std::runtime_error` and adds a line number attribute to pinpoint the exact location of the error.

### 2. `TypeEnv`
- **Role**: Manages the type environment for variables, including their names and types.
- **Why**: Ensures correct variable scoping and resolution, preventing conflicts between local and global variables.
- **Overview**:
  - **Attributes**:
    - `vars`: A map storing variable names and their corresponding types.
    - `parent`: A shared pointer to the parent environment, enabling inheritance.
  - **Methods**:
    - `define`: Adds a new variable to the current environment.
    - `resolve`: Retrieves the type of a variable, searching up the hierarchy if not found locally.

### 3. `TypeChecker`
- **Role**: Performs static type checking on the Abstract Syntax Tree (AST).
- **Why**: Validates the AST against the defined type system, ensuring that all operations are performed correctly.
- **Overview**:
  - **Attributes**:
    - `globalEnv`: A shared pointer to the global type environment.
  - **Methods**:
    - `check`: Entry point for type checking a list of AST nodes.
    - `checkNode`: Recursively checks a single AST node and returns its inferred type.

## Trade-offs

1. **Complexity vs. Usability**: While the type checker enhances the quality of the generated code, it introduces additional complexity into the compiler's implementation. Balancing this complexity with usability is crucial for maintaining developer productivity.

2. **Performance Impact**: Static type checking can add overhead to the compilation process. Optimizing performance while still providing comprehensive type checking is a significant challenge.

3. **Flexibility vs. Strictness**: Allowing for more flexible type systems can lead to less predictable code, whereas stricter type rules ensure consistency but may limit expressiveness. Finding the right balance is essential for a versatile yet reliable language.

By addressing these trade-offs, the QuantumLanguage compiler aims to provide developers with a powerful toolset for building efficient and bug-free quantum applications.