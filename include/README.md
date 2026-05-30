# QuantumLanguage Compiler - TypeChecker.h

## Overview

The `include/TypeChecker.h` header file is an integral component of the QuantumLanguage compiler, focusing on the static type checking phase. This phase ensures that the source code adheres to the defined type rules before actual compilation begins. The `TypeChecker` class plays a crucial role in managing the type environment (`TypeEnv`) and performing type checks on Abstract Syntax Tree (AST) nodes.

## Role in Compiler Pipeline

In the QuantumLanguage compiler pipeline, the `TypeChecker.h` file serves as the second step after parsing the source code into an AST. It performs static type checking to validate the types used in the program. If any type errors are detected during this stage, it throws a `StaticTypeError`, which includes the error message and the line number where the error occurred. This allows developers to catch and fix type-related issues early in the development process, improving code quality and reducing potential runtime errors.

## Key Design Decisions and Why

1. **Separation of Concerns**: The type checker is designed as a separate module to maintain a clean separation between lexical analysis, syntax analysis, and semantic analysis. This modular approach makes the compiler easier to understand, test, and extend.

2. **Use of Exceptions**: By throwing exceptions for type errors, the type checker can provide clear and actionable feedback to the developer. This helps in identifying and correcting type-related issues efficiently.

3. **Dynamic Type Environment**: The `TypeEnv` struct uses a shared pointer to its parent environment, allowing for a dynamic hierarchy of scopes. This feature is essential for handling nested functions, loops, and conditional blocks where variable types may change or be different depending on the scope.

4. **Recursive Type Checking**: The `checkNode` function recursively traverses the AST to perform type checking on each node. This ensures that all parts of the program are checked for type consistency, covering complex control structures and function calls.

## Major Classes/Functions Overview

### Class: `StaticTypeError`
- Inherits from `std::runtime_error`.
- Adds a line number attribute to store the location of the error in the source code.
- Used to throw exceptions when a static type error is detected.

### Struct: `TypeEnv`
- Represents the type environment, which tracks variable names and their associated types.
- Contains a map of variables to their types and a shared pointer to the parent environment.
- Provides methods to define new variables and resolve existing ones, with a fallback to "any" type if not found.

### Class: `TypeChecker`
- Manages the overall type checking process.
- Initializes with a global type environment.
- Offers public methods to check entire ASTs or individual nodes.
- Utilizes the `checkNode` function to perform recursive type checking.

## Tradeoffs

1. **Performance vs. Accuracy**: While static type checking provides early error detection, it can sometimes lead to false positives or missed errors due to limitations in the type system. Balancing performance with thoroughness requires careful consideration of the type rules and how they are enforced.

2. **Complexity vs. Usability**: A more complex type system can offer better safety guarantees but at the cost of increased complexity in both the implementation and usage of the language. Simplifying the type system can make the language easier to use but might reduce its robustness.

3. **Memory Usage**: Storing the type environment and maintaining its hierarchy can consume significant memory, especially for large programs with many nested scopes. Optimizing memory usage while preserving the necessary information is a challenge.

By addressing these tradeoffs, the QuantumLanguage compiler aims to provide a balance between type safety, usability, and performance, making it a reliable tool for developing quantum applications.