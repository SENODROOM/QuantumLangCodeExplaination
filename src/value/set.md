# `set` Function

## Overview

The `set` function is a member method of the `Value` class in the Quantum Language compiler. It assigns a new value to a variable or constant within the current scope. If the variable is defined as a constant, an error is thrown to prevent reassignment. If the variable is not found in the current scope but exists in a parent scope, the assignment is propagated to the parent scope. If neither the current nor the parent scope contains the variable, a `NameError` is thrown indicating that the variable is undefined.

## Parameters

- `name`: A string representing the name of the variable or constant to be assigned a new value.
- `val`: The new value to be assigned to the variable or constant. This can be of any type supported by the Quantum Language compiler.

## Return Value

This function does not return a value (`void`). It updates the value of the specified variable or constant within the current scope or propagates the assignment to the parent scope.

## Edge Cases

1. **Constant Reassignment**: Attempting to assign a new value to a constant results in a `RuntimeError`. This ensures that constants retain their initial values throughout the program execution.
2. **Parent Scope Assignment**: If the variable is not found in the current scope but exists in a parent scope, the assignment is made in the parent scope. This allows variables to be accessible across different scopes without explicit declaration in each scope.
3. **Undefined Variable**: If the variable is neither found in the current nor the parent scope, a `NameError` is thrown. This helps catch errors early in the development process, ensuring all variables are properly declared before use.

## Interactions with Other Components

- **Scope Management**: The `set` function interacts with the scope management system to determine where the variable or constant should be updated. It uses the `vars` map to store local variables and the `constants` set to identify constants.
- **Cell Synchronization**: For variables stored in shared cells (both pointer and reference cases), the `set` function also updates the value in the corresponding cell using the `cells` map. This ensures that changes to the variable are reflected in all places where it is accessed.

Here's a breakdown of how the function works:

1. **Local Variable Check**:
   - The function first checks if the variable or constant named `name` exists in the current scope by looking up its entry in the `vars` map.
   - If the variable exists, it further checks whether it is a constant by looking it up in the `constants` set.
   - If the variable is a constant, a `RuntimeError` is thrown, preventing reassignment.
   - If the variable is not a constant, its value is updated in the `vars` map.

2. **Parent Scope Check**:
   - If the variable is not found in the current scope, the function checks if there is a parent scope by verifying if the `parent` pointer is not null.
   - If a parent scope exists, the function calls the `set` method on the parent scope, passing the same `name` and `val`.
   - This allows assignments to propagate up the scope hierarchy, making variables accessible across different scopes.

3. **Undefined Variable Error**:
   - If neither the current nor the parent scope contains the variable, a `NameError` is thrown, indicating that the variable is undefined.
   - This helps catch errors related to undeclared variables, ensuring proper variable declarations before use.

By handling these cases and interacting with other components like scope management and cell synchronization, the `set` function provides robust support for variable assignment in the Quantum Language compiler, ensuring correct behavior and preventing common programming errors.