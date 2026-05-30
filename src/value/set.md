# `set` Function

## Overview

The `set` function is a member method of the `Value` class in the Quantum Language compiler. This function is responsible for assigning a new value to a variable or constant within the current scope. If the variable is defined as a constant, an error is thrown to prevent reassignment.

### Parameters
- `name`: A string representing the name of the variable or constant to be assigned a new value.
- `val`: The new value to be assigned, which can be any type supported by the `Value` class.

### Return Value
- None (`void`)

### Edge Cases
1. **Reassignment of Constants**: If the variable being assigned is a constant, attempting to reassign it will result in a `RuntimeError`. This ensures that constants retain their initial values throughout the execution of the program.
2. **Variable Not Found in Current Scope**: If the variable is not found in the current scope but exists in a parent scope, the function will delegate the assignment to the parent scope. If the variable does not exist anywhere in the scope hierarchy, a `NameError` will be thrown indicating that the variable is undefined.

### Interactions with Other Components
- **Local Variables Map (`vars`)**: The function first checks if the variable exists in the local variables map (`vars`). If it does, it updates the value associated with the variable. If the variable is also marked as a constant in the `constants` set, a `RuntimeError` is thrown to prevent reassignment.
- **Shared Cells Map (`cells`)**: If the variable is found in the `cells` map, it indicates that the variable is either a pointer or a reference to another location in memory. In such cases, the function updates the value at the location pointed to by the shared cell.
- **Parent Scope Handling**: If the variable is not found in the current scope, the function checks if there is a parent scope. If a parent scope exists, it delegates the assignment to the parent scope using recursion. If no parent scope is found, a `NameError` is thrown.

This design allows the `set` function to handle assignments in a hierarchical manner, ensuring that constants remain immutable and that variables correctly reflect changes across different scopes.