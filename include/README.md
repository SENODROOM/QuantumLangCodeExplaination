# QuantumLanguage Compiler - TypeChecker.h

## Overview

The `include/TypeChecker.h` header file is an essential component of the QuantumLanguage compiler's core infrastructure. It primarily focuses on the static type checking phase of the compilation process, ensuring that the program adheres to its type definitions before execution begins. The file defines several key data structures and classes necessary for type resolution and validation during the compilation of abstract syntax trees (ASTs).

This header file plays a critical role in maintaining the integrity and correctness of the compiled quantum programs by enforcing strict type constraints and providing mechanisms for error handling related to type mismatches.

## Key Design Decisions

### Use of Exceptions for Error Handling

**Why:** In the context of static type checking, it is often more appropriate to halt compilation and report errors immediately rather than continuing with potentially incorrect code. Using exceptions allows for clear and immediate signaling of type-related issues, facilitating easier debugging and maintenance.

### Hierarchical Type Environment (`TypeEnv`)

**Why:** To support nested scopes and variable shadowing, a hierarchical type environment was designed. Each `TypeEnv` instance can have a parent, allowing for lookup of variables in enclosing scopes. This structure simplifies the management of variable types across different parts of the program without requiring complex lookups or duplications.

## Classes and Functions Documentation

### Class: `StaticTypeError`

**Purpose:** Represents a static type error encountered during the compilation process. Inherits from `std::runtime_error`.

**Behavior:** Constructs an exception with a message and the line number where the error occurred. Provides access to the line number through the `line` member variable.

### Struct: `TypeEnv`

**Purpose:** Manages the type environment for the current scope, including variable definitions and resolving variable types based on their scope.

**Behavior:**
- **Constructor:** Initializes a new `TypeEnv` instance with an optional parent environment.
- **Method `define`:** Adds a variable definition to the current environment.
- **Method `resolve`:** Recursively resolves the type of a variable by searching the current environment and its parents. If the variable is not found, returns "any" as a fallback type.

### Class: `TypeChecker`

**Purpose:** Performs static type checking on the abstract syntax tree (AST) of a quantum program.

**Behavior:**
- **Constructor:** Initializes a new `TypeChecker` instance with a global type environment.
- **Method `check`:** Checks the entire list of AST nodes for type consistency.
- **Method `checkNode`:** Recursively checks a single AST node for type consistency, using the provided type environment.

## Tradeoffs and Limitations

### Exception Overhead

Using exceptions for error handling introduces overhead compared to traditional error codes. However, given the importance of type safety in static compilation, this tradeoff is deemed acceptable for its clarity and ease of use.

### Fallback Type "Any"

The fallback type "any" is used when a variable's type cannot be resolved. While this provides flexibility, it also means that type safety is compromised at runtime, which could lead to unexpected behavior or errors.

### Limited Support for Dynamic Types

The current implementation primarily supports static typing. Extending support for dynamic types would require significant changes and additional complexity to the type checker.

### No Support for Polymorphism

Polymorphic features such as method overloading and inheritance are not currently supported. Adding these features would involve substantial modifications to both the type checker and the AST representation.

## Conclusion

The `include/TypeChecker.h` header file is a fundamental part of the QuantumLanguage compiler responsible for ensuring type safety during the static analysis phase. Its design choices, while introducing some tradeoffs, provide a robust framework for managing variable types and detecting errors early in the development process. By understanding these components and their implications, developers can better appreciate the intricacies involved in building a reliable quantum programming language compiler.