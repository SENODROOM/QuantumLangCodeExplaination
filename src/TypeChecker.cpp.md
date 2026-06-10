# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` serves as a crucial part of the Quantum Language compiler, responsible for static type checking of the Abstract Syntax Tree (AST). This process ensures that all expressions and statements adhere to their declared types, thereby preventing potential runtime errors due to type mismatches.

## Role in the Compiler Pipeline

The `TypeChecker` operates during the semantic analysis phase of the compilation process. It traverses the AST, starting from the root and moving down through each node, applying type rules to ensure consistency. The results of the type checking are used to generate more accurate code representations and to provide early feedback on any issues related to data types.

## Key Design Decisions and Why

1. **Global Environment**: A `globalEnv` shared pointer of `TypeEnv` is initialized to store built-in functions and variables. This environment is used as the base for type resolution throughout the program.

2. **Recursive Traversal**: The `check` function is designed to handle both individual nodes and blocks of statements recursively. This allows for comprehensive type checking across all parts of the AST.

3. **Dynamic Initialization**: For variable declarations (`VarDecl`), the initial value's type is determined dynamically. If a type hint is provided, it overrides the inferred type, ensuring explicitness where necessary.

4. **Built-in Function Handling**: Built-in functions like `print`, `input`, `len`, `sha256`, and `aes128` are predefined in the global environment. Their types are set to `"any"` or specific types based on their functionality.

## Major Classes/Functions Overview

- **TypeChecker Class**:
  - **Constructor**: Initializes the `globalEnv` with built-in functions and variables.
  - **check Method**: Takes a vector of AST nodes and checks each one using the `checkNode` method.
  
- **checkNode Function**:
  - Recursively checks an AST node and its children.
  - Handles different types of nodes such as literals, identifiers, variable declarations, function declarations, and binary expressions.
  - Provides warnings for type mismatches between declared and actual types.

## Tradeoffs

- **Complexity vs. Usability**: While providing detailed type information can help catch errors early, it also increases the complexity of the type checker. Balancing thoroughness with practical usability is a challenge.
  
- **Performance**: Recursive traversal of the AST can lead to performance overhead, especially for large programs. Optimizing the traversal algorithm while maintaining correctness is essential.

- **Flexibility vs. Strictness**: Allowing dynamic initialization and type hints provides flexibility, but it can also lead to less strict type enforcement. Finding the right balance between these aspects is crucial for effective type checking.

By addressing these tradeoffs, `TypeChecker.cpp` aims to provide robust static type checking that enhances the reliability and maintainability of the compiled quantum programs.