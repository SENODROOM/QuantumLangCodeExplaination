# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` is a critical component of the Quantum Language compiler, responsible for performing static type checking on the Abstract Syntax Tree (AST). This essential step ensures that all expressions and statements adhere to their specified types, thereby mitigating runtime errors due to type mismatches.

## Role in the Compiler Pipeline

The `TypeChecker` operates during the semantic analysis phase of the compilation process. Specifically, it traverses the AST and checks the types of various nodes such as variables, functions, and binary expressions. By enforcing type constraints, the `TypeChecker` helps maintain the integrity and correctness of the program before actual execution begins.

## Key Design Decisions and Justifications

### Environment Management
**Decision:** The `TypeChecker` uses an environment (`TypeEnv`) to manage variable and function scopes.
**Justification:** This approach allows for easy tracking of variable types throughout different parts of the program. It also supports nested scopes, which are necessary for handling local variables within functions.

### Built-in Function Handling
**Decision:** Built-in functions like `print`, `input`, `len`, `sha256`, and `aes128` are predefined with specific types.
**Justification:** Predefining these functions simplifies the type checking process by providing known types for common operations. This avoids unnecessary complexity and potential errors in user-defined implementations.

### Dynamic Typing Support
**Decision:** The `TypeChecker` supports dynamic typing through the use of the `"any"` type hint.
**Justification:** Quantum programming often requires flexibility in data types, especially when dealing with quantum states and operations. Allowing `"any"` type hints provides a balance between strict type safety and practicality in certain scenarios.

## Documentation of Major Classes/Functions

### Class: TypeChecker
**Purpose:** Manages the type checking process for the entire AST.
**Behaviour:** Initializes the global type environment and iterates through the AST nodes, calling `checkNode()` to validate each node's type.

### Function: check()
**Purpose:** Checks the types of nodes within a given list of AST nodes.
**Behaviour:** Iterates through each node in the provided list and calls `checkNode()` to validate its type. Handles both individual nodes and blocks of statements.

### Function: checkNode()
**Purpose:** Validates the type of a single AST node.
**Behaviour:** Depending on the node type, performs different checks:
- **NumberLiteral**: Returns `"float"`.
- **StringLiteral**: Returns `"string"`.
- **BoolLiteral**: Returns `"bool"`.
- **Identifier**: Resolves the identifier's type using the current environment.
- **VarDecl**: Checks the type of the variable declaration against its initializer (if any) and updates the environment accordingly.
- **FunctionDecl**: Defines the function's parameters and body within a new scope and validates its return type.
- **BlockStmt**: Creates a new scope for the block and recursively checks each statement within it.

## Tradeoffs and Limitations

### Flexibility vs. Strictness
**Tradeoff:** The support for dynamic typing via `"any"` type hints offers flexibility but can lead to less predictable behavior at runtime.
**Limitation:** Users must be cautious when using `"any"` to avoid unintended type mismatches.

### Scope Management Complexity
**Tradeoff:** While managing nested scopes enhances type checking accuracy, it adds complexity to the implementation.
**Limitation:** Properly handling scoping rules is crucial but can introduce bugs if not implemented correctly.

### Built-in Functions
**Tradeoff:** Predefining built-in functions simplifies type checking but may limit customization options.
**Limitation:** Users cannot redefine built-in functions with different types, which could be useful in some advanced use cases.

## Conclusion

`TypeChecker.cpp` plays a vital role in ensuring the type safety of the Quantum Language programs. Through careful design decisions and comprehensive functionality, it effectively prevents runtime errors and maintains the integrity of the compiled code. However, users should be aware of its limitations and tradeoffs, particularly regarding dynamic typing and scope management.