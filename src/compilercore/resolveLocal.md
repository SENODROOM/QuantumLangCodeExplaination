# `resolveLocal` Function

## Overview

The `resolveLocal` function is an essential utility method in the Quantum Language compiler's `CompilerCore.cpp` file. Its primary purpose is to locate and return the index of a local variable within the current lexical scope by its name. This function aids in resolving variable references during compilation, ensuring that variables are accessed correctly based on their declared positions.

## Parameters

- `state`: A pointer to the current compilation state, which includes information about the current lexical scope and the list of local variables.
- `name`: The name of the local variable whose index needs to be resolved.

## Return Value

- Returns the index of the local variable if found within the current lexical scope.
- Returns `-1` if the local variable is not found within the current lexical scope.

## Edge Cases

- If the `name` parameter is empty or null, the function will return `-1`, indicating that the variable name is invalid.
- If there are multiple local variables with the same name but declared in different scopes, only the first occurrence in the current scope will be returned.
- If the current scope does not contain any local variables, the function will return `-1`.

## Interactions with Other Components

The `resolveLocal` function interacts with the following components:

- **Compilation State (`state`)**: It accesses the `locals` vector within the `state` object to search for the local variable by name.
- **Scope Management**: By iterating over the `locals` vector in reverse order, starting from the most recently declared variable, the function ensures that the nearest declaration is prioritized, adhering to the principle of lexical scoping.

This interaction allows the `resolveLocal` function to effectively manage variable resolution within nested scopes, providing accurate indices for subsequent operations such as variable access and assignment.