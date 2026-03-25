# `set` Function

## Overview

The `set` function is a member method of the `Value` class in the Quantum Language compiler. It assigns a new value to a variable or constant within the current scope. If the variable is defined as a constant, an error is thrown to prevent reassignment. If the variable is not found in the current scope, the function checks its parent scope. If the parent also does not contain the variable, a `NameError` is thrown indicating that the variable is undefined.

## Parameters/Return Value

- **Parameters**:
  - `name`: A string representing the name of the variable or constant to be assigned a new value.
  - `val`: The new value to be assigned. This can be of various types depending on the context in which the `Value` class is used.

- **Return Value**: 
  - None (`void`). The function updates the value of the specified variable or constant directly.

## How It Works

1. **Locate Variable in Current Scope**:
   - The function first attempts to find the variable or constant named `name` in the `vars` map, which represents the local variables and constants in the current scope.

2. **Check for Constant**:
   - If the variable is found in the `vars` map, the function then checks if `name` is also present in the `constants` set. If it is, a `RuntimeError` is thrown because constants cannot be reassigned.

3. **Update Local Variable**:
   - If the variable is found and is not a constant, the function updates the value of the variable in the `vars` map to `val`.

4. **Sync with Shared Cells**:
   - To ensure consistency across different parts of the program that might share the same variable, the function also checks if there is a corresponding entry in the `cells` map. If such an entry exists, it dereferences the shared cell pointer and assigns `val` to the memory location pointed to by the shared cell.

5. **Delegate to Parent Scope**:
   - If the variable is not found in the current scope, the function checks if there is a parent scope (`parent`). If a parent scope exists, it calls the `set` function on the parent scope with the same arguments (`name` and `val`).

6. **Throw Error for Undefined Variable**:
   - If the variable is not found in either the current scope nor its parent, a `NameError` is thrown, indicating that the variable is undefined.

## Edge Cases

- **Reassigning Constants**: Attempting to reassign a value to a constant will result in a `RuntimeError`. This ensures that once a constant is initialized, its value remains unchanged throughout the execution of the program.
  
- **Variable Not Found in Current or Parent Scope**: If the variable is not found in either the current or parent scope, a `NameError` is thrown. This prevents accidental modification of undeclared variables, ensuring that all variable modifications are intentional and declared.

- **Shared Cell Consistency**: By updating the value in the `cells` map, the function ensures that any part of the program that shares the same variable through pointers or references sees the updated value immediately.

## Interactions with Other Components

- **Scope Management**: The `set` function interacts with the scope management system of the compiler. It uses the `vars`, `constants`, and `cells` maps to track and manage variables and their values efficiently.

- **Error Handling**: The function relies on error handling mechanisms provided by the compiler, specifically throwing `RuntimeError` and `NameError` when appropriate conditions are met.

- **Memory Management**: When dealing with shared cells, the function indirectly interacts with the memory management subsystem, ensuring that changes made to the shared cell are reflected in all parts of the program that reference it.