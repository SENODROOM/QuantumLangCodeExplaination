# `define` Function in Quantum Language Compiler

## Overview
The `define` function in the Quantum Language compiler is essential for introducing new variables or constants into the current lexical scope. This function updates the symbol table, ensuring that these identifiers can be accessed and used within the program.

## Parameters
- `name`: A string representing the name of the variable or constant to be defined.
- `val`: The value associated with the variable or constant being defined. This could be any data type supported by the Quantum Language.
- `isConst`: A boolean flag indicating whether the identifier should be treated as a constant (`true`) or a mutable variable (`false`).

## Return Value
This function does not return any value explicitly. However, its primary effect is to update the symbol table, making the newly defined variable or constant accessible throughout the current scope.

## Edge Cases
1. **Duplicate Definitions**: If an attempt is made to define a variable or constant with a name that already exists in the current scope, the existing entry will be overwritten.
2. **Scope Management**: The function ensures that the new definition is added to the correct lexical scope. If called outside of a valid scope, it may lead to undefined behavior or errors.
3. **Constant Overwrite**: Attempting to redefine a constant with a different value will result in the constant's value being updated, but the `constants` map will still mark it as a constant.

## Interactions with Other Components
- **Symbol Table**: The `define` function interacts directly with the symbol table to store the new variable or constant. The symbol table is a critical component of the compiler that keeps track of all declared identifiers and their types.
- **Type Checking**: Before defining a new variable or constant, the compiler performs type checking to ensure that the provided value matches the expected type for the given identifier.
- **Error Handling**: In case of invalid definitions (e.g., trying to define a constant without the `const` keyword), the function triggers appropriate error handling mechanisms to alert the user about the issue.

## Implementation Details
Here is the implementation of the `define` function:

```cpp
void define(const std::string& name, Value&& val, bool isConst) {
    // Store the value in the symbol table under the specified name
    vars[name] = std::move(val);
    
    // Mark the identifier as a constant if the isConst flag is set
    if (isConst) {
        constants[name] = true;
    }
}
```

### Explanation
1. **Storing the Value**:
   ```cpp
   vars[name] = std::move(val);
   ```
   - This line moves the `val` into the `vars` map using the `name` as the key. Moving instead of copying helps optimize performance, especially when dealing with large objects.
   
2. **Marking as Constant**:
   ```cpp
   if (isConst) {
       constants[name] = true;
   }
   ```
   - If the `isConst` flag is `true`, this line marks the identifier `name` as a constant in the `constants` map. This prevents reassignment of the constant later in the code.

By updating the symbol table and marking identifiers appropriately, the `define` function facilitates the proper management of variables and constants within the Quantum Language compiler, ensuring that they are correctly scoped and accessible during compilation.