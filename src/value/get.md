# `get` Function

## Overview
The `get` function is an essential method in the Quantum Language compiler used to fetch the value of a variable specified by its name (`name`). This function guarantees that variables are accessible within their designated lexical scope and efficiently navigates upwards through the scope chain if necessary.

## Parameters
- **name**: A string representing the name of the variable whose value needs to be retrieved.

## Return Value
- Returns the value associated with the variable named `name`.
- If the variable is not found in the current scope or any of its parent scopes, throws a `NameError`.

## Edge Cases
1. **Variable Not Found**: The function will throw a `NameError` if the variable being sought is not present in either the local `vars` map or any of the parent scopes.
2. **Scope Chain Traversal**: If the variable is not found in the local scope, the function will recursively traverse up the scope chain until it finds the variable or reaches the global scope without success.

## Interactions with Other Components
- **cells**: This component holds live values that have been directly assigned using the `&` operator. The `get` function checks this map first to ensure that any direct assignments are considered before falling back to regular variable lookups.
- **vars**: This map stores the values of variables declared within the current scope. If the variable is found here, its value is returned immediately.
- **parent**: This member points to the parent scope object. If the variable is not found in the current scope, the `get` function calls the `get` method on the parent scope to continue searching. This allows for nested scoping and variable resolution across multiple levels.

This comprehensive design ensures that the `get` function operates efficiently and accurately, providing access to variables as they exist at runtime while maintaining clear boundaries between different scopes.