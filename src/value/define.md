# `define` Function in Quantum Language Compiler

## Overview
The `define` function in the Quantum Language compiler is essential for introducing new variables or constants into the current lexical scope. This function updates both the `vars` map and optionally the `constants` set within the compiler's symbol table to ensure that these identifiers can be referenced correctly throughout the code.

### Why It Works This Way
By updating the `vars` map, the `define` function allows the compiler to keep track of all variable names and their associated values within the current scope. The use of `std::move` ensures efficient transfer of ownership of the value object to the map, preventing unnecessary copying.

When a constant is defined (`isConst` is true), the function also adds the name to the `constants` set. This helps in distinguishing between mutable variables and immutable constants during subsequent operations, such as type checking and optimization.

### Parameters/Return Value
- **Parameters**:
  - `name`: A string representing the name of the variable or constant being defined.
  - `val`: An rvalue reference to the value object being assigned to the variable or constant.
  - `isConst`: A boolean indicating whether the identifier should be treated as a constant.

- **Return Value**: None. The function modifies the internal state of the compiler's symbol table directly.

### Edge Cases
1. **Duplicate Definitions**: If an attempt is made to redefine a variable or constant within the same scope, the existing entry will be overwritten.
2. **Scope Management**: The function assumes that the current scope is properly managed by the compiler. If the scope is not correctly set, the behavior of the function may be unpredictable.
3. **Type Safety**: While the function itself does not enforce type safety, relying on proper usage ensures that the types of variables and constants are consistent and valid.

### Interactions with Other Components
- **Symbol Table**: The `define` function interacts closely with the symbol table component, which manages all identifiers in the program. By updating the symbol table, it enables other parts of the compiler to resolve references to these identifiers accurately.
- **Type Checker**: During type checking, the `constants` set helps in determining whether an identifier can be modified. If an identifier is marked as a constant, any attempts to modify its value will result in a type error.
- **Optimizer**: The `constants` set can also be used by the optimizer to identify expressions that can be evaluated at compile time, potentially improving performance.

In summary, the `define` function plays a critical role in managing the symbol table of the Quantum Language compiler, ensuring that variables and constants are introduced correctly and efficiently. Its interaction with other components like the type checker and optimizer further enhances the overall functionality and robustness of the compiler.