# `define` Function in Quantum Language Compiler

## Overview
The `define` function is used within the Quantum Language compiler to introduce new variables or constants into the current lexical scope. This function is essential for managing the symbol table and enabling access to these identifiers throughout the program.

### Parameters
- `name`: A string representing the name of the variable or constant being defined.
- `val`: A value object that holds the data associated with the variable or constant.
- `isConst`: A boolean flag indicating whether the identifier should be treated as a constant (i.e., its value cannot be changed after definition).

### Return Value
This function does not return any value; it is a void function.

### Edge Cases
1. **Duplicate Definitions**: If an attempt is made to define an identifier that already exists in the current scope, the behavior is undefined and may result in runtime errors or unexpected behavior.
2. **Scope Management**: The function must correctly manage the scope hierarchy, ensuring that identifiers are only accessible within their intended scope and not leaking into outer scopes unintentionally.
3. **Memory Management**: Proper memory management is required to avoid leaks when defining values that hold dynamically allocated resources.

### Interactions with Other Components
- **Symbol Table**: The `define` function interacts with the symbol table to store the new variable or constant. It updates both the general `vars` map and the `constants` map if the identifier is marked as a constant.
- **Lexical Analysis**: During lexical analysis, tokens are identified and passed to the `define` function to create new identifiers. The function ensures that these identifiers are properly scoped and managed.
- **Semantic Analysis**: After the `define` function has added new identifiers to the symbol table, semantic analysis can proceed to check the types and usage of these identifiers according to the language's rules.

### Example Usage
Here is an example of how the `define` function might be used within the compiler:

```cpp
void define(const std::string& name, Value val, bool isConst) {
    // Store the variable or constant in the symbol table
    vars[name] = std::move(val);

    // Mark the identifier as a constant if necessary
    if (isConst) {
        constants[name] = true;
    }
}

// Example call during compilation
define("pi", 3.14159, true); // Define pi as a constant
define("counter", 0, false); // Define counter as a mutable variable
```

In this example, the `define` function is called twice to add two new identifiers to the symbol table: `pi`, which is a constant with the value `3.14159`, and `counter`, which is a mutable variable initialized to `0`. The function ensures that these identifiers are stored correctly and appropriately flagged as constants or variables.