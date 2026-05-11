# `resolveLocal` Function

## Overview

The `resolveLocal` function is a utility method utilized within the Quantum Language compiler's `CompilerCore.cpp` file. This function primarily serves to identify and retrieve the index of a local variable within the current lexical scope based on its name. If the specified variable is not found, the function returns `-1`.

### Why It Works This Way

The implementation of `resolveLocal` iterates through the list of local variables in reverse order. This approach ensures that the most recently declared variable is returned first, adhering to the Last In, First Out (LIFO) principle commonly associated with stack-based languages. By starting from the end of the list and moving towards the beginning, the function can quickly locate the variable without having to traverse the entire list.

### Parameters/Return Value

- **Parameters**:
  - `name` (const std::string&): The name of the local variable to be resolved.

- **Return Value**:
  - `int`: The index of the local variable if found; otherwise, `-1`.

### Edge Cases

- **Empty Scope**: If there are no local variables in the current scope (`state->locals.empty()`), the function will return `-1`.
- **Variable Not Found**: If the specified variable name does not exist among the local variables in the current scope, the function will also return `-1`.
- **Multiple Declarations**: In scenarios where multiple variables share the same name but are declared at different scopes, only the most recent declaration will be identified.

### Interactions with Other Components

The `resolveLocal` function interacts closely with the `CompilerState` object, which maintains information about the current compilation state including the list of local variables. This interaction allows the function to access the necessary data to perform its resolution task accurately.

Here is a more detailed breakdown of the function:

```cpp
int CompilerCore::resolveLocal(const std::string& name) {
    // Iterate through the locals vector in reverse order
    for (int i = static_cast<int>(state->locals.size()) - 1; i >= 0; --i) {
        // Check if the current local variable matches the given name
        if (state->locals[i].name == name) {
            // Return the index of the matching local variable
            return i;
        }
    }
    // Return -1 if the variable is not found
    return -1;
}
```

In summary, the `resolveLocal` function is an essential utility for locating local variables within the current scope. Its LIFO iteration strategy ensures efficient resolution, while its interaction with the `CompilerState` object provides the necessary context to perform accurate lookups.