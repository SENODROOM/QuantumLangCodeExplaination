# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` is a crucial component of the Quantum Language compiler responsible for static type checking within the Abstract Syntax Tree (AST). This ensures that all expressions and statements adhere to their specified types, thereby mitigating runtime errors due to type mismatches.

## Role in the Compiler Pipeline

The `TypeChecker` class plays a pivotal role in the compilation process. It traverses the AST, performing type checks at each node. The primary steps include:

1. **Initialization**: The constructor initializes the global environment with predefined functions and their return types.
2. **Traversal**: The `check` function recursively traverses the AST, calling `checkNode` for each node.
3. **Type Checking**: `checkNode` performs the actual type checking based on the node's type:
   - For literals (`NumberLiteral`, `StringLiteral`, `BoolLiteral`), it returns their respective types.
   - For identifiers (`Identifier`), it resolves the type from the current environment.
   - For variable declarations (`VarDecl`), it checks if the initializer matches the provided type hint and defines the variable in the environment.
   - For function declarations (`FunctionDecl`), it sets up a new environment for parameters and checks the body of the function.
   - For blocks (`BlockStmt`), it creates a new environment and checks each statement within the block.

## Key Design Decisions and Why

1. **Global Environment**: A shared pointer to a `TypeEnv` object is used as the global environment, allowing modifications across different scopes without copying data.
2. **Recursive Traversal**: The use of recursion simplifies the traversal of nested structures within the AST.
3. **Dynamic Typing with 'Any'**: To accommodate flexibility, the compiler uses 'any' as a default type, which can be overridden by explicit type hints or inferred from initializers.
4. **Type Mismatch Warnings**: Instead of strict error handling, warnings are issued for potential type mismatches, encouraging developers to refine their code while still allowing some flexibility.

## Major Classes/Functions Overview

### Class: TypeChecker

- **Constructor**: Initializes the global environment with built-in functions.
- **Function: check**
  - Takes a vector of AST nodes and iterates through them, calling `checkNode`.
- **Function: check**
  - Overloaded version that takes a single AST node and handles blocks specifically.
- **Function: checkNode**
  - Recursively checks an individual AST node and its children.
  - Handles various node types including literals, identifiers, variable declarations, function declarations, and blocks.

### Class: TypeEnv

- Represents the type environment where variables and functions are defined.
- Provides methods to define and resolve types.

## Tradeoffs

1. **Flexibility vs. Strictness**:
   - Using 'any' allows for more flexible code but may lead to subtle bugs.
   - Warnings instead of errors provide a balance between enforcing type safety and accommodating developer needs.
   
2. **Performance**:
   - Recursive traversal can be computationally expensive for large ASTs.
   - Efficient implementation of environments minimizes overhead but adds complexity.

3. **Readability vs. Complexity**:
   - The design aims for readability and simplicity in the main logic.
   - However, the need to handle multiple node types and maintain environments introduces additional complexity.

Overall, `TypeChecker.cpp` is a well-designed module that balances flexibility, performance, and developer experience, ensuring robust type safety in the Quantum Language compiler.