# `get` Function

## Overview
The `get` function is a critical method within the Quantum Language compiler that retrieves the value of a variable specified by its name (`name`). This function facilitates access to variables within the current lexical scope and seamlessly navigates up the scope hierarchy if the variable is not found in the local scope.

## Parameters
- `name`: A string representing the name of the variable whose value needs to be retrieved.

## Return Value
- The value associated with the variable specified by `name`. If the variable exists in the current scope or any parent scope, the corresponding value is returned.
- Throws an exception of type `NameError` if the variable is undefined in both the current and parent scopes.

## Edge Cases
- **Local Variable**: If the variable is defined locally within the current scope, the function returns its value immediately without checking parent scopes.
- **Parent Scope**: If the variable is not found in the local scope but is defined in one of the parent scopes, the function recursively calls itself on the parent scope until the variable is found or all parent scopes have been checked.
- **Undefined Variable**: If the variable is neither found in the local nor any parent scope, the function throws a `NameError` indicating that the variable is undefined.

## Interactions with Other Components
- **Scope Management**: The `get` function interacts with the scope management system to keep track of variables defined in different scopes. It uses two main data structures:
  - `cells`: A map that stores pointers to live values of variables. This is useful when a variable's value has been modified using the address-of operator (`&var`).
  - `vars`: A map that stores the actual values of variables.
- **Recursive Call**: When the variable is not found in the current scope, the function makes a recursive call to the `get` method of the parent scope. This ensures that the function can traverse the entire scope hierarchy to find the variable.
- **Exception Handling**: If the variable is undefined after checking all scopes, the function throws a `NameError`, which is caught and handled appropriately by the caller. This helps maintain robust error handling throughout the compiler.

## Implementation Details
Here is the implementation of the `get` function:

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
    
    // Recursively check parent scope
    if (parent)
        return parent->get(name);
    
    // Throw an error if the variable is undefined
    throw NameError("Undefined variable: '" + name + "'");
}
```

### Explanation of Code
1. **Check `cells` Map**:
   - The function first checks if the variable is present in the `cells` map. This map contains pointers to live values of variables, which might have been modified using the address-of operator.
   - If the variable is found in `cells`, the function dereferences the pointer and returns the value.

2. **Check Local Variables**:
   - If the variable is not found in `cells`, the function then checks the `vars` map, which contains the actual values of variables.
   - If the variable is found in `vars`, the function returns its value.

3. **Recursively Check Parent Scope**:
   - If the variable is still not found in either `cells` or `vars`, the function checks if there is a parent scope.
   - If a parent scope exists, the function makes a recursive call to the `get` method of the parent scope, passing the same `name`.
   - This process continues until the variable is found or all parent scopes have been checked.

4. **Throw `NameError`**:
   - If the variable is undefined after checking all scopes, including the parent scopes, the function throws a `NameError` with a message indicating that the variable is undefined.
   - This exception is typically caught and handled by higher-level parts of the compiler to provide meaningful error messages to the user.

This implementation ensures that the `get` function can effectively retrieve variable values across multiple scopes, providing a comprehensive solution for accessing variables in the Quantum Language compiler.