# `set` Function

## Overview

The `set` function is a member method of the `Value` class in the Quantum Language compiler. This function is responsible for assigning a new value to a variable or constant within the current scope. If the variable is defined as a constant, an error is thrown to prevent reassignment.

### Parameters
- `name`: A string representing the name of the variable or constant to be assigned a new value.
- `val`: The new value to be assigned, which can be of any type supported by the `Value` class.

### Return Value
- None (`void`)

### Edge Cases
1. **Reassignment of Constants**: Attempting to assign a new value to a constant will result in a `RuntimeError`. This ensures that constants remain immutable throughout the program execution.
2. **Variable Not Found**: If the variable name provided does not exist in the current scope and there is no parent scope, a `NameError` will be thrown indicating that the variable is undefined.

### Interactions with Other Components
- **Local Variables Map (`vars`)**: The `set` function updates the local variables map directly using the variable name as the key. If the variable exists in this map, its value is updated.
- **Shared Cells Map (`cells`)**: If the variable is also present in the shared cells map, the function synchronizes the change by updating the value pointed to by the shared cell. This handles both pointer and reference cases where multiple parts of the program might need to access the same data.
- **Parent Scope**: If the variable is not found in the current scope but exists in a parent scope, the function recursively calls `set` on the parent scope. This allows nested scopes to modify variables defined in outer scopes.

### Why It Works This Way
This implementation ensures that:
- Variables and constants have clear distinctions, preventing accidental modification of constants.
- Changes made in one part of the program reflect in others when appropriate, maintaining consistency across different parts of the code.
- The function gracefully handles errors related to undefined variables, providing clear feedback to the programmer.

By following this approach, the `set` function maintains the integrity and predictability of the program's state while allowing for flexible and dynamic modifications within various scopes.