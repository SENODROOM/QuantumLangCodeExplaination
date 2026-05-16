# Quantum Language Compiler - TypeChecker.cpp

## Overview

`TypeChecker.cpp` is a crucial part of the Quantum Language compiler, responsible for static type checking of the Abstract Syntax Tree (AST). It ensures that all expressions and statements adhere to their declared types, thereby mitigating runtime errors due to type mismatches.

## Role in the Compiler Pipeline

The `TypeChecker` plays a pivotal role in the compilation process. After parsing and building the AST, the type checker traverses the tree to validate the types of variables, function parameters, and return values. If any type inconsistencies are detected, it raises warnings or errors accordingly. This step precedes code generation, ensuring that the subsequent stages receive valid, well-typed input.

## Key Design Decisions and Why

1. **Global Environment**: The `TypeChecker` maintains a global environment (`globalEnv`) where built-in functions and constants are defined. This allows for easy access and validation of these entities during the type-checking phase.

2. **Sub-environments**: For compound structures like blocks within functions, the type checker uses sub-environments (`subEnv`). These encapsulate local variable bindings and help maintain type consistency across different scopes.

3. **Basic Type Checking**: The implementation includes basic checks for literal types (numbers, strings, booleans), identifiers, and variable declarations. This ensures that simple data types are correctly validated.

4. **Advanced Type Checks**: More complex checks are performed for binary expressions, function calls, and control flow statements. These checks ensure that operations are performed between compatible types and that function calls match their parameter types.

## Major Classes/Functions Overview

### `TypeChecker`
- **Constructor**: Initializes the global environment with predefined built-in functions and constants.
- **Public Methods**:
  - `check(const std::vector<ASTNodePtr>& nodes)`: Iterates through a list of AST nodes and checks each one.
  - `check(const ASTNodePtr& node)`: Checks a single AST node, handling nested blocks appropriately.
  - `checkNode(const ASTNodePtr& node, std::shared_ptr<TypeEnv> env)`: Recursively checks an AST node within a given environment.

### `TypeEnv`
- Represents a symbol table that maps variable names to their types.
- Provides methods to define new symbols, resolve existing ones, and handle scoping.

### `Colors`
- A utility class for colored output in error messages, enhancing readability and user experience.

## Trade-offs

1. **Complexity vs. Accuracy**: While more advanced type checking can catch subtle errors, it also increases the complexity of the type checker. Balancing thoroughness with performance is a challenge.

2. **Performance**: Recursive traversal of the AST can be computationally expensive. Optimizing the type checker to minimize redundant checks and improve caching mechanisms is necessary for efficient compilation.

3. **Flexibility vs. Strictness**: Allowing implicit type conversions ("any") provides flexibility but can lead to unexpected behavior. Striking a balance between strict adherence to declared types and practical usability is essential.

By carefully managing these aspects, `TypeChecker.cpp` contributes significantly to the reliability and robustness of the Quantum Language compiler, ensuring that only well-formed programs proceed to the next stages of compilation.