# `get` Function

## Overview
The `get` function is a crucial method in the Quantum Language compiler designed to retrieve the value of a variable identified by its name (`name`). This function ensures that variables remain accessible within their defined lexical scope and efficiently traverses upwards if necessary to locate the variable's value.

## Parameters
- **name**: A string representing the name of the variable whose value needs to be fetched.

## Return Value
- The value associated with the variable named `name`. If the variable exists in either the current or any parent scope, its value is returned. If the variable is not found, an exception of type `NameError` is thrown indicating that the variable is undefined.

## Edge Cases
1. **Variable Not Found**: If the variable is not present in the current scope or any parent scopes, the function will throw a `NameError`.
2. **Scope Hierarchy**: The function correctly handles nested scopes by checking both the local `cells` map and the `vars` map before delegating the search to the parent scope.

## Interactions with Other Components
- **Cells Map**: The function first checks the `cells` map to see if there is a live value associated with the variable name. The `cells` map stores pointers to values that have been modified using the reference operator (`&`).
- **Vars Map**: If the variable is not found in the `cells` map, the function then searches the `vars` map, which contains the original values of all variables declared in the current scope.
- **Parent Scope**: If the variable is still not found in the `vars` map, the function checks if there is a parent scope. If a parent scope exists, the function recursively calls the `get` method on the parent scope to attempt to find the variable.
- **Exception Handling**: If the variable is not found at any level, the function throws a `NameError`, signaling that the variable is undefined. This exception is handled appropriately by the calling code to prevent further execution and provide meaningful error messages to the user.

## Implementation Details
```cpp
{
    // Check cells first: if a pointer has written through &var, cells holds the live value
    auto cit = cells.find(name);
    if (cit != cells.end())
        return *cit->second;
    
    // Check local variables
    auto it = vars.find(name);
    if (it != vars.end())
        return it->second;
    
    // Delegate to parent scope if available
    if (parent)
        return parent->get(name);
    
    // Throw exception if variable is undefined
    throw NameError("Undefined variable: '" + name + "'");
}
```
- The function starts by searching the `cells` map for a live value associated with the variable name. If found, it dereferences the pointer and returns the value.
- If the variable is not found in the `cells` map, the function proceeds to check the `vars` map for the original value of the variable.
- If the variable is still not found, and a parent scope exists, the function recursively calls the `get` method on the parent scope.
- If the variable remains undefined after checking all possible locations, the function throws a `NameError` with a message indicating the undefined variable.

This implementation ensures that the `get` function can effectively locate and return the value of a variable, even when dealing with nested scopes and modified references.