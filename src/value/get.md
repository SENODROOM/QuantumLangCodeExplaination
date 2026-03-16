# `get` Function

## Overview
The `get` function is a method utilized within the Quantum Language compiler to fetch the value of a variable specified by its name (`name`). This function plays a pivotal role in accessing variables within the current lexical scope and traversing up the scope hierarchy if the variable isn't found locally.

### Why It Works This Way
1. **Local Variable Search**: The function first checks the `cells` map to see if there's a live value associated with the variable name. If `cells` contains an entry for `name`, it returns the dereferenced value pointed to by the cell.
2. **Global Variable Search**: If the variable isn't found in `cells`, the function then searches the `vars` map, which stores global variables. If a match is found, it returns the corresponding value.
3. **Parent Scope Traversal**: If neither `cells` nor `vars` contain the variable, the function checks if there is a parent scope (`parent`). If a parent exists, it calls the `get` function on the parent scope, allowing the search to continue up the scope chain.
4. **Exception Handling**: If the variable is still not found after checking all possible locations, the function throws a `NameError` indicating that the variable is undefined.

## Parameters/Return Value
- **Parameters**:
  - `name`: A string representing the name of the variable whose value needs to be retrieved.
  
- **Return Value**:
  - Returns the value of the variable as stored in either the local `cells` or `vars` maps, or as returned by the recursive call to `parent->get(name)`.

## Edge Cases
1. **Variable Not Found**: If the variable is not present in any of the local or parent scopes, the function will throw a `NameError`.
2. **Empty Parent Scope**: If the current scope does not have a parent scope (`parent == nullptr`), and the variable is not found in the local scope, the function will also throw a `NameError`.

## Interactions With Other Components
- **Scope Management**: The `get` function interacts closely with the scope management system, utilizing both the local `cells` and `vars` maps to store and retrieve variable values.
- **Recursive Calls**: When the variable is not found in the current scope, the function makes a recursive call to the `get` method of the parent scope, ensuring that the search continues up the scope chain until the variable is found or an exception is thrown.
- **Error Handling**: The `get` function relies on error handling mechanisms provided by the Quantum Language compiler to manage cases where a variable is undefined.

This comprehensive approach ensures that the `get` function can effectively locate and return the value of a variable across different scopes, providing robust support for variable access within the compiler.