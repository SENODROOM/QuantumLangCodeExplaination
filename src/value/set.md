# `set` Function

## Overview

The `set` function is a member method of the `Value` class in the Quantum Language compiler. This function is responsible for assigning a new value to a variable or constant within the current scope. If the variable is defined as a constant, an error is thrown to prevent reassignment.

### Parameters

- `name`: A string representing the name of the variable or constant to be assigned.
- `val`: The new value to assign to the specified variable or constant.

### Return Value

This function does not return any value (`void`).

### Edge Cases

1. **Reassignment of Constants**: If the variable being assigned is a constant, the function throws a `RuntimeError`. This ensures that constants remain immutable throughout the program execution.
2. **Unassigned Variables**: If the variable being assigned has not been previously defined in the current scope, the function checks if there is a parent scope. If a parent scope exists, the function attempts to assign the value to the variable in the parent scope. If the parent scope also does not contain the variable, a `NameError` is thrown indicating that the variable is undefined.

### Interactions with Other Components

- **Local Variable Storage**: The `set` function updates the `local_vars` map with the new value. This map stores variables defined in the current scope.
- **Shared Cell Synchronization**: If the variable is associated with a shared cell (either a pointer or reference), the function synchronizes the shared cell with the new value. This ensures that all references to the shared cell reflect the updated value.
- **Parent Scope Handling**: If the variable is not found in the current scope but exists in a parent scope, the function delegates the assignment to the parent scope. This allows for hierarchical variable management where child scopes can modify values defined in their parent scopes.

### Implementation Details

Here's a breakdown of how the `set` function operates:

1. **Search for Local Variable**:
   ```cpp
   auto it = vars.find(name);
   ```
   The function first searches for the variable in the `vars` map, which represents the local variables in the current scope.

2. **Check for Constant Definition**:
   ```cpp
   if (it != vars.end())
   {
       if (constants.count(name))
           throw RuntimeError("Cannot reassign constant '" + name + "'");
   ```
   If the variable is found, the function then checks if it is defined as a constant using the `constants` set. If it is a constant, a `RuntimeError` is thrown because constants cannot be reassigned.

3. **Update Local Variable**:
   ```cpp
       it->second = val; // update local vars map
   ```
   If the variable is not a constant, its value is updated in the `vars` map.

4. **Synchronize Shared Cells**:
   ```cpp
       auto cit = cells.find(name);
       if (cit != cells.end())
           *cit->second = val;
   ```
   The function then checks if the variable is associated with a shared cell. If so, it updates the value of the shared cell with the new value.

5. **Delegate to Parent Scope**:
   ```cpp
   }
   else if (parent)
   {
       parent->set(name, std::move(val));
       return;
   }
   ```
   If the variable is not found in the current scope and a parent scope exists, the function calls the `set` method on the parent scope, passing along the variable name and new value.

6. **Throw Error for Undefined Variable**:
   ```cpp
   throw NameError("Undefined variable: '" + name + "'");
   ```
   If the variable is not found in either the current scope or the parent scope, a `NameError` is thrown indicating that the variable is undefined.

By following these steps, the `set` function ensures that variables and constants are managed correctly within the scope hierarchy of the Quantum Language compiler, maintaining immutability and allowing for proper variable assignment and synchronization across different scopes.