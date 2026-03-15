# QuantumLanguage Compiler - TypeChecker.h

## Overview

The `include/TypeChecker.h` header file is an essential part of the QuantumLanguage compiler, focusing on the type checking mechanism. This file defines the `StaticTypeError`, `TypeEnv`, and `TypeChecker` classes, which work together to ensure that the syntax tree adheres to the specified type rules during compilation. The primary role of this file is to validate the types of variables, function parameters, and return values throughout the program, preventing runtime errors related to type mismatches.

## Key Design Decisions

### Use of Exceptions for Error Handling

**Why:** The decision to use exceptions (`std::runtime_error`) for error handling was made to provide clear and structured feedback about static type errors. This approach allows the compiler to propagate error information up the call stack, making it easier to pinpoint the exact location where the error occurred (line number). It also aligns with common practices in C++ for handling exceptional conditions.

### Hierarchical Type Environment

**Why:** A hierarchical type environment (`TypeEnv`) was designed to support nested scopes in the quantum language. Each scope has its own type environment, and when a variable is not found in the current scope, the compiler looks up the parent scope. This design ensures that local variables shadow outer variables, maintaining consistency with how scoping works in other programming languages.

### Shared Pointer for Type Environment

**Why:** Using `std::shared_ptr` for the parent environment allows for efficient memory management and sharing of type environments between different parts of the compiler without copying them. This is particularly useful for large programs with many nested scopes.

## Class Documentation

### StaticTypeError

**Purpose:** Represents a static type error encountered during compilation.

**Behavior:** Inherits from `std::runtime_error` and adds a line number attribute to indicate where the error occurred.

**Tradeoffs:** While exceptions are powerful, they can lead to performance overhead and complex error handling logic. However, for a compiler, static type errors are critical issues that need immediate attention, making exceptions a suitable choice.

### TypeEnv

**Purpose:** Manages the type environment for a given scope, including variable definitions and resolution.

**Behavior:** 
- Contains a map of variable names to their types.
- Supports hierarchical scoping through a shared pointer to the parent environment.
- Provides methods to define new variables and resolve existing ones.

**Tradeoffs:** This design simplifies the implementation of scoping but may increase complexity when dealing with large programs. Additionally, using shared pointers can introduce reference counting overhead, which might be significant in a high-performance compiler.

### TypeChecker

**Purpose:** Performs type checking on the abstract syntax tree (AST) of the quantum language program.

**Behavior:** 
- Initializes with a global type environment.
- Offers methods to check entire ASTs or individual nodes.
- Utilizes the type environment to validate types during traversal of the AST.

**Tradeoffs:** Centralizing the type checking logic in a single class makes the compiler easier to understand and maintain. However, it might limit parallelism in type checking operations, especially for very large programs.

## Usage Example

Below is a simplified example demonstrating how these components might be used together:

```cpp
#include "TypeChecker.h"

int main() {
    // Create a vector of AST nodes representing the program
    std::vector<ASTNodePtr> nodes = ...;

    // Initialize the type checker
    TypeChecker tc;

    try {
        // Check the entire AST
        tc.check(nodes);
    } catch (const StaticTypeError& e) {
        // Handle static type errors
        std::cerr << "Static type error at line " << e.line << ": " << e.what() << std::endl;
    }

    return 0;
}
```

This example shows the initialization of the `TypeChecker` and the attempt to check an AST, with proper error handling for static type errors.