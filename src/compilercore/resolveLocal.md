# `resolveLocal` Function

## Overview

The `resolveLocal` function is a utility method within the Quantum Language compiler's `CompilerCore.cpp` file. Its primary purpose is to locate and return the index of a local variable in the current scope based on its name. If the variable is not found, it returns `-1`.

## Parameters

- **name**: A string representing the name of the local variable to be resolved.

## Return Value

- **int**: The index of the local variable if found, otherwise `-1`.

## How It Works

The function iterates over the list of local variables stored in the `state` object, starting from the last element added (to ensure that the most recently declared variable takes precedence). For each local variable, it checks if the variable's name matches the provided `name`. If a match is found, it immediately returns the index of that variable. If no match is found after iterating through all local variables, the function returns `-1`.

### Why This Approach?

This approach ensures that the most recently declared variable shadows any previously declared variables with the same name. This behavior is crucial in languages like C++, where variable scoping rules allow for shadowing, meaning that a variable declared within an inner scope can hide a variable declared in an outer scope.

## Edge Cases

1. **Empty Scope**: If there are no local variables in the current scope (`state->locals.empty()`), the function will correctly return `-1`.
2. **Variable Not Found**: If the variable name provided does not exist among the local variables, the function will return `-1`.
3. **Case Sensitivity**: The comparison between variable names is case-sensitive. This means that searching for "x" will not find a variable named "X".

## Interactions With Other Components

- **Scope Management**: The `resolveLocal` function interacts with the scope management system, which maintains a list of local variables (`state->locals`). Each time a new variable is declared, it is added to this list.
- **Error Handling**: When a variable is not found, the function returns `-1`, which can be used by higher-level functions to report an error or handle the situation appropriately.

Here is the code snippet for reference:

```cpp
{
    for (int i = static_cast<int>(state->locals.size()) - 1; i >= 0; --i)
        if (state->locals[i].name == name)
            return i;
    return -1;
}
```

In summary, the `resolveLocal` function efficiently resolves local variable indices by leveraging reverse iteration and direct comparison, ensuring correct handling of variable shadowing and providing a robust mechanism for error reporting when necessary.