# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` serves as a crucial part of the Quantum Language compiler, responsible for static type checking on the Abstract Syntax Tree (AST). This ensures that all expressions and statements adhere to their declared types, thereby mitigating runtime errors due to type mismatches.

## Role in the Compiler Pipeline

The `TypeChecker` plays a pivotal role in the compiler's pipeline. It operates after the parser has constructed the AST but before code generation begins. The primary responsibilities include:
- **Basic Types**: Checking literals such as numbers, strings, and booleans.
- **Identifiers**: Resolving variable and function identifiers to their respective types within the current scope.
- **Variable Declarations**: Ensuring that variables are declared with the correct type, either explicitly or implicitly based on initialization.
- **Function Declarations**: Verifying that functions have correctly typed parameters and return values.
- **Block Statements**: Recursively checking each statement within a block for consistency.

## Key Design Decisions and Why

1. **Scope Management**:
   - **Nested Environments**: Each block creates a new environment (`TypeEnv`) that inherits from its parent, allowing for local variable declarations and type resolution.
   - **Why**: This approach ensures that type information is accessible at compile time and respects the scoping rules of the language.

2. **Built-in Functions**:
   - **Predefined Types**: Built-in functions like `print`, `input`, `len`, `sha256`, and `aes128` are predefined with specific return types.
   - **Why**: This simplifies type inference and error handling, ensuring that calls to these functions are always valid.

3. **Type Inference**:
   - **Implicit Typing**: Variables can be inferred to have a type based on their initializers. If an explicit type hint is provided, it overrides the inferred type.
   - **Why**: Implicit typing reduces verbosity and improves readability, while explicit hints provide additional safety and clarity.

4. **Expression Evaluation**:
   - **Recursive Checking**: Binary expressions are checked recursively to ensure both operands are compatible with the operation being performed.
   - **Why**: This allows for complex expression evaluations to be validated without manual intervention, ensuring correctness and consistency.

## Major Classes/Functions Overview

### `TypeChecker`

- **Constructor**: Initializes the global environment with predefined types.
- **Methods**:
  - `check(const std::vector<ASTNodePtr>& nodes)`: Checks all nodes in a vector.
  - `check(const ASTNodePtr& node)`: Handles individual nodes, including blocks.
  - `checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)`: Performs the actual type checking for a given node and environment.

### `TypeEnv`

- **Purpose**: Manages type information for variables and functions within a scope.
- **Methods**:
  - `define(const std::string& name, const std::string& type)`: Adds a new variable or function to the environment.
  - `resolve(const std::string& name)`: Retrieves the type associated with a variable or function.

### `ASTNodePtr`

- **Purpose**: Represents a pointer to an abstract syntax tree node.
- **Usage**: Used throughout the type checker to traverse and validate the AST.

## Trade-offs

- **Complexity vs. Simplicity**: While nested environments add complexity to the implementation, they significantly simplify the type checking logic by respecting scoping rules automatically.
- **Readability vs. Flexibility**: Implicit type inference enhances readability but may limit flexibility in certain scenarios where explicit type hints are necessary.
- **Performance vs. Accuracy**: Recursive checking ensures accuracy but may introduce performance overhead compared to iterative approaches.

Overall, `TypeChecker.cpp` is a well-designed component that balances simplicity, readability, and robustness, playing a vital role in ensuring the type safety of the Quantum Language programs.