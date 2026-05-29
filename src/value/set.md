# `set` Function

## Overview

The `set` function is a member method of the `Value` class in the Quantum Language compiler. This function is responsible for assigning a new value to a variable or constant within the current scope. If the variable is defined as a constant, an error is thrown to prevent reassignment.

### Parameters
- **name**: A string representing the name of the variable or constant to be assigned a new value.
- **val**: The new value to be assigned, which can be of any type supported by the compiler.

### Return Value
- **None**: The function does not return any value.

### Edge Cases
1. **Reassignment of Constants**: If the variable being assigned is a constant, attempting to reassign it will result in a `RuntimeError`. This ensures that constants retain their initial values throughout the program execution.
2. **Variable Not Found**: If the variable being assigned is not found in the current scope and there is no parent scope, a `NameError` will be thrown indicating that the variable is undefined.

### Interactions with Other Components
- **Local Variables Map (`vars`)**: The function first checks if the variable exists in the local variables map. If found, it updates the value associated with the variable.
- **Shared Cells Map (`cells`)**: If the variable is also present in the shared cells map, the function syncs the new value to all live instances of the shared cell. This handles both pointer and reference cases where multiple parts of the program might need to access the same data.
- **Parent Scope**: If the variable is not found in the current scope but exists in a parent scope, the function recursively calls the `set` method on the parent scope. This allows for nested scopes and hierarchical variable management.

### Implementation Details
```cpp
void Value::set(const std::string& name, Value&& val) {
    // Check if the variable exists in the local variables map
    auto it = vars.find(name);
    if (it != vars.end()) {
        // Check if the variable is a constant
        if (constants.count(name)) {
            throw RuntimeError("Cannot reassign constant '" + name + "'");
        }
        // Update the local variables map
        it->second = std::move(val);
        
        // Sync to any live shared cell (covers both pointer and ref cases)
        auto cit = cells.find(name);
        if (cit != cells.end()) {
            *cit->second = std::move(val);
        }
        return;
    }
    
    // If the variable is not found in the current scope, check the parent scope
    if (parent) {
        parent->set(name, std::move(val));
        return;
    }
    
    // Throw an error if the variable is undefined
    throw NameError("Undefined variable: '" + name + "'");
}
```

This implementation ensures that the `set` function correctly handles variable assignments, including checking for constants and syncing values across shared cells. It also provides a mechanism for handling nested scopes through recursive calls to the parent scope's `set` method.