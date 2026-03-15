# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` is a crucial part of the Quantum Language compiler, focusing on static type checking within the Abstract Syntax Tree (AST). This phase ensures that all expressions and statements comply with the specified types, thus mitigating potential runtime errors caused by type inconsistencies.

## Role in the Compiler Pipeline

The `TypeChecker` operates during the semantic analysis stage of the compilation process. It iterates through the AST nodes, applying type rules to validate their correctness. The primary responsibilities include:

- **Variable Declarations**: Ensuring that variables are declared with the correct type.
- **Function Declarations**: Verifying that function parameters and return values match their specified types.
- **Expressions**: Checking that binary and unary operations involve compatible types.
- **Statements**: Validating the structure and type consistency of control flow statements like loops and conditionals.

## Key Design Decisions

### Environment Management

**Why**: To maintain scope-specific type information, allowing nested scopes to have their own variable definitions without affecting outer scopes.

**Decision**: Each `BlockStmt` creates a new environment (`TypeEnv`) inheriting from the parent environment. Variables are defined in these environments, ensuring proper scoping.

### Built-in Function Handling

**Why**: To provide basic functionality without requiring explicit declarations in user code.

**Decision**: Built-in functions such as `print`, `input`, `len`, `sha256`, and `aes128` are predefined with generic types ("any"). This allows them to handle various data types while maintaining simplicity.

### Type Inference

**Why**: To reduce verbosity and allow developers to focus more on logic rather than type annotations.

**Decision**: For variables without explicit type hints, the initial value's type determines the variable's type. However, this approach can lead to implicit type conversions which might not always be desirable.

## Documentation of Major Classes/Functions

### Class: TypeChecker

**Purpose**: Manages the overall type checking process, traversing the AST and applying type rules.

**Behavior**:
- Initializes with a global type environment containing built-in functions.
- Provides methods to check individual nodes and entire blocks of statements.

### Function: check(const std::vector<ASTNodePtr>& nodes)

**Purpose**: Checks multiple AST nodes sequentially.

**Behavior**:
- Iterates over each node in the provided vector.
- Calls `checkNode` to perform type checking on each node.

### Function: check(const ASTNodePtr& node)

**Purpose**: Recursively checks an AST node based on its type.

**Behavior**:
- If the node is a `BlockStmt`, it recursively checks each statement within the block.
- Otherwise, it calls `checkNode` directly on the node.

### Function: std::string checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)

**Purpose**: Performs type checking on a single AST node.

**Behavior**:
- Returns the inferred type of the node.
- Handles different types of literals (number, string, bool).
- Resolves identifiers using the current environment.
- Validates variable declarations and function bodies against their type hints.
- Prints warnings for type mismatches.

## Tradeoffs/Limitations

- **Implicit Type Conversions**: Allowing variables to infer their type from initializers can lead to unexpected behavior if not handled carefully.
- **Generic Types for Built-ins**: Using "any" for built-in functions simplifies implementation but may obscure type safety issues.
- **Limited Scope for Complex Types**: Current implementation supports basic types only, making it challenging to handle complex data structures like arrays or custom objects.

This README provides a comprehensive overview of the `TypeChecker.cpp` file, detailing its role, design decisions, and documentation of key components.