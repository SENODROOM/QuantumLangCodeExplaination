# `set` Function

## Overview

The `set` function is a member method of the `Value` class in the Quantum Language compiler. It assigns a new value to a variable or constant within the current scope. If the variable is defined as a constant, an error is thrown to prevent reassignment. If the variable is not found in the current scope but exists in a parent scope, the assignment is propagated upwards. If the variable is neither found in the current nor any parent scopes, an error is raised indicating that the variable is undefined.

### Parameters
- `name`: A string representing the name of the variable or constant to be assigned a new value.
- `val`: The new value to be assigned, which can be of various types depending on the context.

### Return Value
- This function does not explicitly return a value (`void`). However, it updates the internal state of the `Value` object by assigning a new value to a variable or constant.

### Edge Cases
1. **Reassignment of Constants**: Attempting to reassign a value to a constant results in a `RuntimeError`. This ensures that constants remain immutable after their initial assignment.
2. **Undefined Variable**: If the variable is not found in the current or any parent scope, a `NameError` is thrown, indicating that the variable has not been declared before.

### Interactions with Other Components
- **Scope Management**: The `set` function interacts with the scope management system of the compiler. It uses the `vars` map to store variables and the `cells` map to manage shared memory cells (both pointers and references).
- **Parent Scope Propagation**: When a variable is not found in the current scope, the function checks if it exists in the parent scope. If so, it propagates the assignment to the parent scope using the `parent->set(name, std::move(val))` call.
- **Shared Memory Synchronization**: If a variable is associated with a shared memory cell (found in the `cells` map), the function synchronizes the change by updating the value pointed to by the shared cell (`*cit->second = val`).

This implementation ensures that assignments are handled correctly across different scopes and maintains immutability for constants, providing robustness and predictability in the language's execution environment.