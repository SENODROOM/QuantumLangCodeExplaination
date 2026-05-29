# `define` Function in Quantum Language Compiler

## Overview
The `define` function in the Quantum Language compiler is essential for introducing new variables or constants into the current lexical scope. This function updates the symbol table to ensure that these identifiers can be accessed and used throughout the program.

### Parameters
- `name`: A string representing the name of the variable or constant to be defined.
- `val`: The value associated with the variable or constant being defined.
- `isConst`: A boolean indicating whether the identifier should be treated as a constant (i.e., its value cannot be changed after definition).

### Return Value
This function does not return any value directly. Its primary effect is to update the internal state of the compiler's symbol table.

### Edge Cases
1. **Duplicate Definitions**: If an attempt is made to redefine a variable or constant within the same lexical scope, the existing entry will be overwritten.
2. **Scope Management**: The function must correctly manage the scope of the identifiers. Variables defined in one block should not conflict with those defined in another.
3. **Constant Values**: When defining a constant (`isConst` set to `true`), any subsequent attempts to modify the value of that constant should result in an error.

### Interactions with Other Components
- **Symbol Table**: The `define` function interacts closely with the symbol table component, which stores all the identifiers and their corresponding values. It updates this table based on the provided `name`, `val`, and `isConst`.
- **Error Handling**: The function may interact with the error handling component to report errors related to duplicate definitions or invalid modifications of constants.
- **Parser**: During the parsing phase, when the compiler encounters a `define` statement, it calls this function to add the new identifier to the symbol table.

Here is a more detailed breakdown of how the function works:

```cpp
void define(const std::string& name, Value&& val, bool isConst) {
    // Check if the variable already exists in the current scope
    if (vars.find(name) != vars.end()) {
        // Handle error: Duplicate definition
        throw std::runtime_error("Duplicate definition of " + name);
    }

    // Add the variable to the symbol table
    vars[name] = std::move(val);

    // If the variable is marked as a constant, record it in the constants map
    if (isConst) {
        constants[name] = true;
    }
}
```

In summary, the `define` function plays a critical role in managing the identifiers within the Quantum Language compiler. It ensures that new variables and constants are properly introduced and scoped, preventing conflicts and allowing for appropriate error handling.