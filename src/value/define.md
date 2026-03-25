# `define` Function in Quantum Language Compiler

## Overview
The `define` function is utilized within the Quantum Language compiler to introduce new variables or constants into the current lexical scope. This function plays a crucial role in managing the symbol table, thereby facilitating the accessibility of these variables and constants across different parts of the program.

### Parameters
- **name**: A string representing the name of the variable or constant being introduced.
- **val**: An rvalue reference to a `Value` object, which encapsulates the data type and value of the variable or constant.
- **isConst**: A boolean flag indicating whether the introduced entity is a constant (`true`) or a mutable variable (`false`).

### Return Value
This function does not return any value explicitly. Its primary purpose is to update the internal state of the compiler's symbol table.

### Edge Cases
1. **Duplicate Definitions**: If an attempt is made to redefine a variable or constant that already exists in the current scope, the behavior is undefined and may result in runtime errors or incorrect compilation outcomes.
2. **Scope Management**: The function must correctly manage the scope of the defined entities. Variables and constants defined within a block should only be accessible within that block, while those defined at a higher level should remain accessible throughout the scope.
3. **Type Safety**: The function ensures that the types of variables and constants match their intended usage, preventing type mismatches during subsequent operations.

### Interactions with Other Components
- **Symbol Table**: The `define` function interacts directly with the symbol table component of the compiler. It updates both the general variable map (`vars`) and the constant map (`constants`) based on the provided parameters.
- **Error Handling**: During the definition process, error handling mechanisms such as checking for duplicate definitions and ensuring type safety are invoked. These mechanisms help in identifying and reporting errors early in the compilation process.
- **Code Generation**: Once a variable or constant is defined, the compiler can generate appropriate code for its use in subsequent instructions or expressions.

### Implementation Details
The implementation of the `define` function is straightforward and involves updating the symbol table:

```cpp
{
    vars[name] = std::move(val);  // Move the value into the variable map
    if (isConst)                  // Check if the entity is a constant
        constants[name] = true;   // Mark the entity as a constant in the constants map
}
```

- **Variable Map Update**: The `std::move(val)` operation moves the `Value` object into the `vars` map under the specified `name`. This ensures efficient memory management and avoids unnecessary copying.
- **Constant Flag Update**: If the `isConst` flag is set to `true`, the corresponding entry in the `constants` map is also updated to mark the entity as a constant. This helps in distinguishing between mutable and immutable entities during subsequent operations.

By understanding how the `define` function operates and its interactions with other components, developers can effectively utilize it to manage variables and constants within the Quantum Language compiler, ensuring correct symbol table maintenance and type safety.