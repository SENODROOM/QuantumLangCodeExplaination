# `resolveLocal` Function

## Overview

The `resolveLocal` function is a utility method used within the Quantum Language compiler's `CompilerCore.cpp` file. Its main functionality is to find and return the index of a local variable in the current scope based on its name. If the variable is not found, it returns `-1`. This function plays a crucial role in managing variables during compilation, ensuring that references to local variables are correctly resolved.

## Parameters

- **`const std::string& name`**: The name of the local variable whose index needs to be resolved.

## Return Value

- **`int`**: The index of the local variable if found; otherwise, `-1`.

## How It Works

The function iterates over the list of local variables (`state->locals`) in reverse order. This approach ensures that the most recently declared variable with the given name is found first, as per the Last-In-First-Out (LIFO) principle. For each variable, it checks if the variable's name matches the provided name using the equality operator (`==`). If a match is found, the function immediately returns the index of that variable. If the loop completes without finding a match, the function returns `-1`, indicating that the variable was not found in the current scope.

## Edge Cases

- **Empty Scope**: If the current scope has no local variables (`state->locals.empty()`), the function will return `-1`.
- **Multiple Variables with the Same Name**: Since the function searches in reverse order, it will return the index of the last variable declared with the same name. This behavior can be useful in some scenarios but may need adjustments depending on the language's rules regarding variable shadowing.
- **Variable Not Found**: If no variable with the specified name exists in the current scope, the function returns `-1`.

## Interactions with Other Components

The `resolveLocal` function interacts with several components within the compiler:

- **Scope Management**: It relies on the `state->locals` vector, which represents the current scope's local variables. This vector is maintained by higher-level functions responsible for handling scopes.
- **Error Handling**: If the variable is not found, the function returns `-1`. However, error handling should be implemented at a higher level to provide meaningful feedback or take appropriate actions when an unresolved variable reference is encountered.

In summary, the `resolveLocal` function is essential for resolving local variable indices in the current scope, facilitating correct variable access during compilation. Its implementation efficiently handles various edge cases and integrates seamlessly with the compiler's scope management system.