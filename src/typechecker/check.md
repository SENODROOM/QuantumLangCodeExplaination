# `check` Function

## Overview

The `check` function plays a crucial role in the Quantum Language compiler's type-checking phase. Its main objective is to ensure that the source code adheres to the language's syntactic and semantic rules. This function operates recursively, examining each node in the abstract syntax tree (AST) to verify its correctness.

### Why It Works This Way

Using a recursive approach allows the `check` function to traverse through all nodes of the AST, ensuring comprehensive validation. Each node represents a part of the source code, such as expressions, statements, or declarations. By recursively checking each node, the function can handle nested structures and maintain context throughout the entire program.

## Parameters

- **nodes**: A vector containing pointers to the nodes of the abstract syntax tree (AST). These nodes represent various elements of the source code, including expressions, statements, and declarations.
  
- **globalEnv**: A reference to the global environment object, which holds information about variables, functions, and types defined at the global scope. This environment is essential for resolving identifiers and validating their usage within the program.

## Return Value

The `check` function returns nothing (`void`). However, during its execution, it raises exceptions if any type-related errors are detected. For example, if an operation is performed on incompatible types, or if an undefined identifier is used, the function will throw an appropriate exception.

## Edge Cases

1. **Empty AST**: If the `nodes` vector is empty, the `check` function will simply return without performing any checks, as there is no source code to validate.

2. **Single Node**: When the `nodes` vector contains only one node, the function will check this single node and then return.

3. **Nested Structures**: The function handles nested structures gracefully by recursively calling itself for each child node. This ensures that even deeply nested constructs are validated correctly.

4. **Global Scope Validation**: The `check` function validates identifiers and types defined in the global scope using the `globalEnv`. This includes verifying that all global variables and functions are declared before they are used.

## Interactions With Other Components

- **Symbol Table**: The `check` function interacts with the symbol table component to resolve identifiers and validate their usage. It uses the `globalEnv` to look up symbols and ensure they are declared and have the correct type.

- **Error Reporting**: During the type-checking process, the `check` function reports errors to the error reporting component. If any type-related issues are found, it throws exceptions, which are caught and reported back to the user.

- **Scope Management**: The function manages scopes by keeping track of the current scope and updating it as it traverses through the AST. This ensures that local variables and functions do not conflict with those in the global scope.

In summary, the `check` function is a vital component of the Quantum Language compiler's type-checking phase. By employing a recursive approach, it comprehensively validates the source code against syntactic and semantic rules, ensuring the correctness and reliability of the compiled quantum programs.