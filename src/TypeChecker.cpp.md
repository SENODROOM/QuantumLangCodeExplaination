# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` is an essential component of the Quantum Language compiler, tasked with performing static type checking on the Abstract Syntax Tree (AST). This process ensures that all expressions and statements conform to their declared types, thus preventing runtime errors caused by type mismatches.

## Role in the Compiler Pipeline

The `TypeChecker` operates during the semantic analysis phase of the compilation process. After parsing and building the AST, the `TypeChecker` traverses the tree to verify the types of variables, function parameters, and return values. If any type inconsistencies are detected, it reports them as warnings or errors, halting the compilation if necessary.

### Key Design Decisions and Why

1. **Global Environment**: The `TypeChecker` maintains a global environment (`globalEnv`) that tracks the types of all identifiers defined throughout the program. This allows for cross-function type resolution without requiring explicit type propagation through every scope.

2. **Sub-environments**: For each block statement, a new sub-environment (`subEnv`) is created. This encapsulates the local scope within the block, ensuring that variable types are correctly resolved relative to their definition points.

3. **Built-in Functions**: Built-in functions like `print`, `input`, `len`, `sha256`, and `aes128` are predefined in the global environment with their respective return types. This simplifies type checking for these functions and ensures they are used correctly throughout the code.

4. **Basic Type Checking**: The `TypeChecker` performs basic type checks for arithmetic operations and equality comparisons. For example, addition and subtraction are only allowed between numbers, while equality checks can be performed between any types.

## Major Classes/Functions Overview

- **TypeChecker Class**:
  - **Constructor**: Initializes the global environment with built-in functions.
  - **check Function**: Entry point for type checking an AST. It iterates over each node in the AST and calls `checkNode`.

- **checkNode Function**:
  - **Purpose**: Recursively checks the type of a given AST node.
  - **Handling Different Node Types**:
    - **NumberLiteral**: Returns `"float"` since all numbers are treated as floating-point literals.
    - **StringLiteral**: Returns `"string"` for string literals.
    - **BoolLiteral**: Returns `"bool"` for boolean literals.
    - **Identifier**: Resolves the identifier's type using the current environment.
    - **VarDecl**: Checks the initializer's type against the user-provided type hint (if any), defines the variable in the environment, and returns `"void"`.
    - **FunctionDecl**: Creates a sub-environment for the function body, checks the types of parameters, and then checks the body itself. Defines the function in the global environment as `"fn"` and returns `"void"`.
    - **BlockStmt**: Creates a sub-environment for each block statement and recursively checks the types of all contained statements.
    - **BinaryExpr**: Checks the types of both operands based on the operation being performed. Ensures valid type combinations for arithmetic and comparison operations.

## Trade-offs

- **Complexity vs. Accuracy**: While maintaining a global environment simplifies type resolution, it adds complexity to managing scope and nested environments. However, this approach ensures accurate type checking across the entire program.

- **Performance vs. Usability**: Creating sub-environments for each block statement improves type accuracy but may impact performance, especially in large programs. Balancing these factors requires careful optimization and profiling.

- **Flexibility vs. Strictness**: Allowing implicit type conversion ("any" type) provides flexibility but can lead to harder-to-debug issues. Enforcing strict type checking would improve code reliability but might limit the expressiveness of the language.

By addressing these trade-offs, `TypeChecker.cpp` ensures robust and efficient type checking, enhancing the overall quality and maintainability of the compiled quantum programs.