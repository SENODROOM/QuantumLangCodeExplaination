# `resolveLocal` Function

## Overview

The `resolveLocal` function is an essential utility method in the Quantum Language compiler's `CompilerCore.cpp` file. Its primary purpose is to locate and return the index of a local variable within the current lexical scope by its name. This function aids in managing variable references during compilation, ensuring that variables are accessed correctly and efficiently.

## Parameters/Return Value

- **Parameters**:
  - `name`: A string representing the name of the local variable to be resolved.
  
- **Return Value**:
  - An integer representing the index of the local variable if found, otherwise `-1`.

## How It Works

The `resolveLocal` function iterates through the list of local variables stored in the `state->locals` vector. It starts from the end of the vector (the most recently declared variable) and moves backward towards the beginning. For each variable, it checks if the variable's name matches the provided `name`. If a match is found, it returns the index of that variable. If no match is found after iterating through all variables, it returns `-1`.

This approach ensures that the most recent declaration of a variable takes precedence over older declarations with the same name, which is crucial for maintaining correct semantics in nested scopes.

## Edge Cases

- **Variable Not Found**: If the specified variable name does not exist in the current lexical scope, the function returns `-1`.
- **Empty Scope**: If the `state->locals` vector is empty, the function will also return `-1`, as there are no local variables to search through.
- **Multiple Declarations**: In scenarios where multiple variables with the same name exist in different scopes, only the most recent one will be returned.

## Interactions with Other Components

The `resolveLocal` function interacts primarily with the `CompilerState` object, which holds information about the current state of the compilation process, including the list of local variables (`state->locals`). This function is typically called when the compiler needs to access or manipulate a local variable during the parsing or code generation stages.

By leveraging the `resolveLocal` function, the compiler can efficiently manage variable references, reducing the complexity of handling nested scopes and avoiding potential errors related to incorrect variable accesses.