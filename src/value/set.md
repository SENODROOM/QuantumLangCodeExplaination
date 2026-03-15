# `set` Function

## Overview

The `set` function is a method within the `Value` class of the Quantum Language compiler. Its primary purpose is to assign a new value to a variable or constant in the current scope or propagate the assignment upwards through the scope hierarchy if the variable is not found in the current scope.

## Parameters

- **name**: A string representing the name of the variable or constant to be assigned a new value.
- **val**: The new value to be assigned. This can be any data type supported by the Quantum Language compiler.

## Return Value

This function does not explicitly return a value but rather updates the state of the `Value` object by modifying its internal variables (`vars`, `cells`, and `constants`). It returns `void`.

## Edge Cases

1. **Reassignment of Constants**: If an attempt is made to reassign a constant, the function throws a `RuntimeError`. This ensures that constants remain immutable throughout the execution of the program.
2. **Variable Not Found**: If the variable being assigned is not found in the current scope and there is no parent scope, the function throws a `NameError`. This indicates that the variable has not been declared anywhere in the scope hierarchy.

## How It Works

1. **Local Variable Check**:
   - The function first checks if the variable named `name` exists in the local `vars` map using `auto it = vars.find(name);`.
   - If the variable is found (`it != vars.end()`), it proceeds to check if the variable is also marked as a constant in the `constants` set.
   - If the variable is a constant, it throws a `RuntimeError` because constants cannot be reassigned.
   - If the variable is not a constant, it updates the value of the variable in the `vars` map to `val` using `it->second = val;`.

2. **Shared Cell Synchronization**:
   - After updating the local variable, the function checks if there is a corresponding entry in the `cells` map using `auto cit = cells.find(name);`.
   - If such an entry exists, it synchronizes the value of the shared cell pointed to by `*cit->second` with the new value `val`.

3. **Parent Scope Propagation**:
   - If the variable named `name` is not found in the local `vars` map, the function checks if there is a parent scope (`if (parent)`).
   - If a parent scope exists, it calls the `set` method on the parent scope, passing along the `name` and `val`.
   - This allows the assignment to propagate up the scope hierarchy until it reaches a scope where the variable is defined.

4. **Undefined Variable Error**:
   - If neither the local `vars` map nor the parent scope contains the variable named `name`, the function throws a `NameError`.
   - This error message indicates that the variable has not been declared anywhere in the scope hierarchy, which is a critical issue in programming.

## Interactions With Other Components

- **Scope Management**: The `set` function interacts with the scope management system by checking the `vars` and `cells` maps to determine if the variable exists and needs to be updated.
- **Constant Handling**: It also interacts with the constant handling system by checking if the variable is a constant and throwing an error if it is attempted to be reassigned.
- **Parent Scope**: When the variable is not found in the current scope, the function delegates the task to the parent scope, allowing for hierarchical scope resolution and propagation of assignments.

In summary, the `set` function is crucial for managing variable assignments in the Quantum Language compiler, ensuring that variables are properly updated and constants remain immutable. It handles local and parent scope interactions and throws appropriate errors when undefined variables are encountered.