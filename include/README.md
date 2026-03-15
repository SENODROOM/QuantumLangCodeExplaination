# QuantumLanguage Compiler - TypeChecker.h

## Overview

The `include/TypeChecker.h` header file is a critical component of the QuantumLanguage compiler, responsible for ensuring type safety during the compilation process. It defines the `StaticTypeError` exception class and the `TypeEnv` structure, as well as the `TypeChecker` class, which performs static type checking on abstract syntax tree (AST) nodes. This file plays a pivotal role in preventing runtime errors related to incorrect data types and helps maintain the integrity of the program's logic.

## Design Decisions

### Use of `std::variant` for Value Encapsulation

**Why:** The decision to use `std::variant` was driven by the need to support multiple data types within the QuantumLanguage. By encapsulating these types within a single variant, we ensure that the interpreter can handle different value types seamlessly without requiring explicit type casting. This approach simplifies the implementation and enhances type safety.

### Hierarchical Type Environment (`TypeEnv`)

**Why:** Implementing a hierarchical type environment allows us to manage variable scoping effectively. Each `TypeEnv` instance represents a scope, and variables defined in one scope are accessible within nested scopes unless shadowed by a variable with the same name in a more local scope. This design facilitates the resolution of variable names and their associated types, ensuring that the correct type is used throughout the program.

### Exception Handling for Static Type Errors

**Why:** Raising exceptions for static type errors provides a clear and structured way to report issues at compile time rather than waiting until runtime. This approach not only improves error reporting but also allows the compiler to halt processing immediately upon encountering a type error, preventing further potential issues.

## Class Documentation

### `StaticTypeError` Exception Class

**Purpose:** Represents a static type error encountered during the compilation process. This exception includes both the error message and the line number where the error occurred.

**Behavior:** Inherits from `std::runtime_error`. Provides a constructor that takes an error message and a line number, initializing the base class with the message and storing the line number.

### `TypeEnv` Structure

**Purpose:** Manages the type environment, including variable definitions and scope resolution.

**Behavior:**
- **Constructor:** Initializes a new `TypeEnv` instance with an optional parent environment.
- **`define` Method:** Adds a variable definition to the current environment.
- **`resolve` Method:** Recursively resolves the type of a variable by searching through the current environment and its parent environments. If the variable is not found, returns "any" as the default type.

### `TypeChecker` Class

**Purpose:** Performs static type checking on AST nodes to ensure type safety.

**Behavior:**
- **Constructor:** Initializes a new `TypeChecker` instance with a global type environment.
- **`check` Methods:** Public methods that take either a vector of AST nodes or a single AST node and initiate the type checking process.
- **`checkNode` Method:** Private method that recursively checks the type of a given AST node within a specific type environment. Returns the inferred type of the node.

## Tradeoffs and Limitations

### Complexity of Type Checking

**Tradeoff:** While using `std::variant` simplifies the implementation of value encapsulation, the complexity of type checking increases due to the need to handle multiple types and ensure consistency across different parts of the AST.

### Limited Support for Advanced Types

**Limitation:** The current implementation supports basic types and uses a simple string-based type representation. Advanced types such as complex numbers, matrices, or custom user-defined types may require additional handling and could complicate the type checker significantly.

### Performance Overhead

**Performance:** The recursive nature of the type checking process, particularly when dealing with deeply nested ASTs, could introduce performance overhead. Optimizations may be needed to improve efficiency, especially for large programs.

## Usage Example

Here's a brief example demonstrating how to use the `TypeChecker` class:

```cpp
#include "TypeChecker.h"

int main() {
    // Create an AST and populate it with nodes
    std::vector<ASTNodePtr> astNodes; // Populate with AST nodes

    // Initialize the type checker
    TypeChecker typeChecker;

    try {
        // Perform type checking
        typeChecker.check(astNodes);
    } catch (const StaticTypeError& e) {
        // Handle static type errors
        std::cerr << "Static type error at line " << e.line << ": " << e.what() << std::endl;
    }

    return 0;
}
```

This example initializes the `TypeChecker`, populates an AST with nodes, and then attempts to perform type checking. If a static type error is encountered, it catches the exception and prints an error message indicating the line number and the error details.

By understanding the purpose and behavior of each class and function in the `TypeChecker.h` file, developers can better integrate type checking into their QuantumLanguage compiler implementations, ensuring robustness and reliability of the compiled programs.