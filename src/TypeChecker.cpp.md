# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` is a critical module within the Quantum Language compiler responsible for static type checking of the Abstract Syntax Tree (AST). It ensures that all expressions and statements adhere to their declared types, thereby preventing runtime errors due to type mismatches.

## Role in the Compiler Pipeline

The `TypeChecker` operates during the semantic analysis phase of the compilation process. After parsing and building the AST, the type checker traverses the tree to validate the types of variables, function parameters, and return values. If any type inconsistencies are detected, it reports them as warnings or errors, ensuring the code's correctness before further processing.

## Key Design Decisions and Why

1. **Global Environment**: The type checker maintains a global environment (`globalEnv`) to store type information for built-in functions and identifiers. This allows for easy resolution of types throughout the program.

2. **Sub-environments**: For each function declaration, a new sub-environment (`subEnv`) is created. This isolates local variable types from the global scope, preventing conflicts and ensuring accurate type inference within functions.

3. **Type Hints**: Function declarations can include type hints for parameters and return values. These hints guide the type checker in validating the types against expected values, providing flexibility while maintaining strong typing.

4. **Basic Type Checks**: Simple type checks are performed for literals like numbers, strings, and booleans. For more complex constructs such as binary expressions and function calls, additional logic is applied to ensure type compatibility.

## Major Classes/Functions Overview

### Class: TypeChecker

- **Constructor**: Initializes the `TypeChecker` with a global environment containing built-in function types.
- **Method: check(const std::vector<ASTNodePtr>& nodes)**: Iterates over a list of AST nodes and checks each one using the `checkNode` method.
- **Method: check(const ASTNodePtr& node)**: Handles individual AST nodes, recursively checking nested blocks and function bodies.

### Function: checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)

- **Purpose**: Validates the type of the given AST node based on its structure and the current environment.
- **Logic**:
  - **Literals**: Returns specific types for number, string, and boolean literals.
  - **Identifiers**: Resolves the type of an identifier from the current environment.
  - **Variable Declarations**: Ensures that the type of a variable matches the initializer's type unless a type hint is provided.
  - **Function Declarations**: Sets up a sub-environment for the function body and validates parameter and return types.
  - **Blocks**: Recursively checks each statement within a block.

## Trade-offs

1. **Flexibility vs. Strictness**: Allowing type hints provides some level of flexibility, but it also increases the complexity of the type checking logic.
   
2. **Performance**: Creating sub-environments for each function adds overhead, which could impact performance, especially in large programs with many functions.

3. **Readability**: While type hints enhance flexibility, they might reduce code readability if not used consistently or appropriately.

Overall, `TypeChecker.cpp` plays a crucial role in ensuring the robustness and reliability of the compiled quantum language code by enforcing strict type constraints during the semantic analysis phase.