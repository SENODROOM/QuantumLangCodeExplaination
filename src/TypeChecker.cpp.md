# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` is a critical component of the Quantum Language compiler, responsible for performing static type checking on the Abstract Syntax Tree (AST). This phase ensures that all expressions and statements adhere to the defined types, thereby preventing runtime errors due to type mismatches.

## Role in the Compiler Pipeline

The `TypeChecker` operates during the semantic analysis stage of the compilation process. Its primary function is to traverse the AST and validate the types of variables, literals, identifiers, function declarations, and binary expressions. By ensuring that types are consistent throughout the program, it helps catch errors early in the development cycle.

## Key Design Decisions

### Environment Management

**Why:** The use of an environment (`TypeEnv`) allows tracking variable and function definitions as well as their types. This is essential for resolving identifiers and verifying type consistency.

**Tradeoff:** Managing environments can become complex, especially with nested scopes. However, this approach provides a robust mechanism for handling type scoping rules.

### Built-in Functions

**Why:** Defining built-in functions like `print`, `input`, `len`, `sha256`, and `aes128` with known types simplifies type inference and error reporting.

**Tradeoff:** Hardcoding built-in function types may limit flexibility in future versions of the language. However, it ensures that these functions are always used correctly.

### Type Inference

**Why:** Inferring types from literals and initializers reduces the need for explicit type annotations, making the code more readable and concise.

**Tradeoff:** Type inference can lead to ambiguous cases where multiple types could be inferred. To mitigate this, the compiler uses basic type checks and warns about potential mismatches.

## Documentation of Major Classes/Functions

### Class: TypeChecker

**Purpose:** Manages the type checking process for the entire AST.

**Behavior:**
- Initializes with a global environment containing built-in functions.
- Traverses the AST and performs type checking on each node.
- Handles different types of nodes such as variables, literals, functions, and blocks.

### Function: check(const std::vector<ASTNodePtr>& nodes)

**Purpose:** Checks a vector of AST nodes.

**Behavior:**
- Iterates through each node in the vector and calls `checkNode`.

### Function: check(const ASTNodePtr& node)

**Purpose:** Checks a single AST node.

**Behavior:**
- If the node is a block statement, recursively checks each statement within the block.
- Otherwise, calls `checkNode` directly.

### Function: checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)

**Purpose:** Checks a specific AST node within a given environment.

**Behavior:**
- Determines the type of the node based on its kind (e.g., number literal, string literal, identifier).
- For variables, checks if the initializer's type matches the declared type (if provided).
- For functions, defines parameters and checks the body within a new scope.
- Returns the type of the node or "void" if not applicable.

## Tradeoffs/Limitations

- **Ambiguity in Type Inference:** While type inference improves readability, it can sometimes lead to ambiguous cases where multiple types could be valid.
- **Limited Flexibility:** Hardcoding built-in function types may restrict future changes without significant refactoring.
- **Basic Type Checking:** The current implementation relies on basic type checks, which might not cover advanced type systems or custom types.

These tradeoffs reflect the balance between simplicity, performance, and functionality required in a compiler's type checker.