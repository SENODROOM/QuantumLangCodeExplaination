# `set` Function

## Overview

The `set` function is a member method of the `Value` class in the Quantum Language compiler. This function is responsible for assigning a new value to a variable or constant within the current scope. If the variable is defined as a constant, an error is thrown to prevent reassignment.

### Parameters
- `name`: A string representing the name of the variable or constant to be assigned a new value.
- `val`: The new value to be assigned, which can be of various types supported by the Quantum Language compiler.

### Return Value
- None (`void`). The function updates the internal state of the `Value` object without returning any value.

### Edge Cases
1. **Variable Not Found**: If the specified variable or constant is not found in the current scope, the function throws a `NameError`. This ensures that only variables that have been declared can be modified.
2. **Constant Reassignment**: Attempting to reassign a constant results in a `RuntimeError`. This prevents accidental modification of constants, maintaining their immutability.
3. **Nested Scopes**: If the variable or constant is not found in the current scope but exists in a parent scope, the function calls the `set` method on the parent scope. This allows for nested scopes where variables can be overridden or extended.

### Interactions with Other Components
- **Local Variables Map (`vars`)**: The function first checks if the variable exists in the local variables map. If found, it updates the value unless the variable is marked as a constant.
- **Shared Cells Map (`cells`)**: If the variable is found in the shared cells map, the function updates the value at the location pointed to by the shared cell. This handles both pointer and reference cases, ensuring that changes propagate across all references to the same data.
- **Parent Scope**: If the variable is not found in the current scope, the function delegates the assignment to the parent scope. This allows for hierarchical variable management, where child scopes can override or extend variables from parent scopes.

### Implementation Details
```cpp
{
    auto it = vars.find(name); // Search for the variable in the local map
    if (it != vars.end()) // Variable found in local scope
    {
        if (constants.count(name)) // Check if the variable is a constant
            throw RuntimeError("Cannot reassign constant '" + name + "'");
        it->second = val; // Update the local variable's value
        // Sync to any live shared cell (covers both pointer and ref cases)
        auto cit = cells.find(name); // Search for the variable in the shared cells map
        if (cit != cells.end())
            *cit->second = val; // Update the value at the shared cell location
        return;
    }
    if (parent) // Variable not found in local scope, check parent scope
    {
        parent->set(name, std::move(val)); // Delegate to parent scope
        return;
    }
    throw NameError("Undefined variable: '" + name + "'"); // Variable not found in any scope
}
```
This implementation ensures that assignments are handled correctly within the scope hierarchy, preventing modifications to constants and propagating changes to shared data locations.