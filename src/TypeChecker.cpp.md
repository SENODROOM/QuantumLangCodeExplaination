# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` is a crucial component of the Quantum Language compiler, responsible for performing static type checking on the Abstract Syntax Tree (AST). This essential step ensures that all expressions and statements adhere to their specified types, thereby mitigating runtime errors due to type mismatches.

## Role in the Compiler Pipeline

The `TypeChecker` operates during the semantic analysis phase of the compilation process. It traverses the AST and checks the types of various nodes such as literals, identifiers, variable declarations, function declarations, and binary expressions. The results of these checks help in generating more accurate and efficient machine code.

## Key Design Decisions

### Use of Shared Pointers for Environment Management

**WHY**: Using shared pointers (`std::shared_ptr`) for managing the environment allows multiple parts of the compiler to share the same environment without copying it. This reduces memory usage and improves performance by avoiding unnecessary duplication of data structures.

### Simplified Function Type Representation

**WHY**: In the current implementation, functions are represented as simply `"fn"` in the environment. This simplification avoids handling complex function types directly, which can complicate the type checking logic. However, this approach limits the ability to differentiate between different function signatures, potentially leading to subtle bugs if not handled carefully.

### Error Handling through Standard Streams

**WHY**: Errors are reported using standard streams (`std::cerr`). This choice provides a straightforward way to output error messages to the console, making it easy for developers to identify issues during the compilation process. However, this method lacks flexibility compared to dedicated error reporting mechanisms, which might be necessary for more advanced tools like IDEs.

## Documentation of Major Classes/Functions

### Class: `TypeChecker`

**Purpose**: Manages the type checking process for the entire AST.

**Behaviour**:
- Initializes the global environment with built-in functions.
- Traverses the AST and performs type checking on each node.
- Handles nested environments for function scopes.

### Function: `check(const std::vector<ASTNodePtr>& nodes)`

**Purpose**: Checks the types of a list of AST nodes.

**Behaviour**:
- Iterates over each node in the provided vector and calls `checkNode`.

### Function: `check(const ASTNodePtr& node)`

**Purpose**: Checks the type of a single AST node.

**Behaviour**:
- If the node is a `BlockStmt`, recursively checks each statement within the block.
- Otherwise, calls `checkNode` directly.

### Function: `checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)`

**Purpose**: Performs type checking on a specific AST node.

**Behaviour**:
- Determines the type of the node based on its kind (e.g., number literal, string literal).
- Resolves identifiers against the provided environment.
- Validates variable declarations and function parameters against their type hints.
- Recursively checks the body of function declarations.

## Tradeoffs/Limitations

- **Simplified Function Types**: The representation of function types as `"fn"` limits the ability to handle overloaded functions or functions with specific parameter types.
- **Standard Stream Error Reporting**: While simple and effective for basic error reporting, this method may not scale well for more sophisticated tools requiring structured error information.
- **Memory Usage**: Although shared pointers reduce memory duplication, they introduce overhead associated with reference counting.

These tradeoffs reflect the balance between simplicity, performance, and feature completeness in the design of the `TypeChecker`.