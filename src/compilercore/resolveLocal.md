# `resolveLocal` Function

## Overview

The `resolveLocal` function is a crucial utility method in the Quantum Language compiler's `CompilerCore.cpp` file. Its primary purpose is to locate and return the index of a local variable within the current lexical scope by its name. This function plays a pivotal role in managing variable references during compilation, ensuring that each variable is correctly accessed and updated.

## Parameters

- **`state`**: A pointer to the current compilation state, which includes information about all local variables defined in the current scope.
- **`name`**: A string representing the name of the local variable whose index needs to be resolved.

## Return Value

- The function returns an integer representing the index of the local variable if found within the current lexical scope.
- If the local variable is not found, the function returns `-1`.

## How It Works

The `resolveLocal` function operates by iterating through the list of local variables stored in the `state->locals` vector in reverse order. This approach ensures that the most recently declared variable takes precedence over any previously declared variables with the same name, adhering to the Last In First Out (LIFO) principle of stack-based variable scoping.

Here’s a step-by-step breakdown of how the function works:

1. **Initialization**: The loop starts from the last element of the `state->locals` vector (`static_cast<int>(state->locals.size()) - 1`) and moves towards the first element (`0`).
2. **Comparison**: For each local variable, the function compares the provided `name` with the `name` attribute of the current local variable.
3. **Index Retrieval**: If a match is found, the function immediately returns the index of the matching local variable.
4. **Completion**: If the loop completes without finding a match, the function returns `-1`, indicating that the local variable is not present in the current scope.

## Edge Cases

- **Variable Not Found**: If the specified variable name does not exist in the current scope, the function will return `-1`. This handles scenarios where a variable reference might be incorrect or out-of-scope.
- **Multiple Declarations**: Since the function iterates in reverse order, it ensures that the most recent declaration of a variable takes precedence. This prevents issues related to shadowing, where a new variable shadows an existing one in the same scope.

## Interactions with Other Components

- **Scope Management**: The `resolveLocal` function interacts closely with the scope management system of the compiler. It relies on the `state->locals` vector, which contains information about all local variables in the current scope.
- **Error Handling**: When the function returns `-1`, it indicates that the variable was not found. This can be used by higher-level functions to generate appropriate error messages or handle missing variable references gracefully.

By understanding the functionality, implementation, and usage of the `resolveLocal` function, developers can better grasp how the Quantum Language compiler manages variable scopes and references during the compilation process.