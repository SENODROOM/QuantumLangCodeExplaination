# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` is a critical component of the Quantum Language compiler, tasked with performing static type checking on the Abstract Syntax Tree (AST). This ensures that all expressions and statements conform to their declared types, thus mitigating potential runtime errors caused by type mismatches.

## Role in the Compiler Pipeline

The `TypeChecker` operates during the semantic analysis phase of the compilation process. After parsing and building the AST, the `TypeChecker` traverses the tree to verify that each expression and statement adheres to its specified type. If any discrepancies are found, it generates appropriate error messages, facilitating early detection and resolution of issues.

## Key Design Decisions and Why

### Environment Management
- **Global and Local Environments**: The `TypeChecker` maintains both global and local type environments. The global environment holds predefined types like built-in functions, while local environments are used for function-specific variables and parameters. This separation allows for accurate type resolution across different scopes.
  
### Type Inference
- **Implicit Types**: For variables without explicit type hints, the `TypeChecker` infers their types based on the initializer expression. This approach reduces boilerplate code and makes the language more user-friendly.

### Dynamic Typing Support
- **Any Type**: The `TypeChecker` supports an "any" type, which can hold values of any other type. This feature is crucial for flexibility in quantum programming where operations might not always have statically determinable types.

## Major Classes/Functions Overview

### Class: TypeChecker
- **Constructor**: Initializes the global type environment with predefined types such as `print`, `input`, etc.
- **Method: check**
  - Accepts a vector of AST nodes and iterates through them, calling `checkNode` for each one.
- **Method: checkNode**
  - Recursively checks individual AST nodes based on their type.
  - Handles various node types including literals, identifiers, variable declarations, function declarations, and binary expressions.

### Class: TypeEnv
- **Purpose**: Manages the current scope's type information.
- **Methods**:
  - `define`: Adds a new symbol to the environment with its associated type.
  - `resolve`: Retrieves the type of a symbol given its name.

## Tradeoffs

### Flexibility vs. Strictness
- **Flexibility**: Supporting dynamic typing (`any`) enhances the language's expressiveness and usability, particularly in complex quantum algorithms.
- **Strictness**: Enforcing strict type checking helps catch errors early in the development process, improving code quality and maintainability.

### Performance vs. Usability
- **Performance**: Predefined types and efficient environment management contribute to better performance during type checking.
- **Usability**: Dynamic typing and flexible syntax improve developer productivity and ease of use.

In conclusion, `TypeChecker.cpp` plays a pivotal role in ensuring the correctness and robustness of the Quantum Language compiler. Its design balances flexibility, strictness, performance, and usability, making it an essential part of the compiler's architecture.