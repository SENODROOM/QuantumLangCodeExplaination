# `get` Function

## Overview
The `get` function is a method used within the Quantum Language compiler to retrieve the value of a variable named `name`. This function is crucial for accessing variables in the current scope and recursively searching up the scope chain if the variable is not found locally.

## Parameters
- **`name`**: A string representing the name of the variable whose value needs to be retrieved.

## Return Value
- The function returns the value associated with the variable `name`.
- If the variable is found in either the local `cells` or `vars` map, the corresponding value is returned.
- If the variable is not found in the local maps but exists in the parent scope, the value from the parent scope is returned.
- If the variable is not defined anywhere in the current or parent scopes, an exception of type `NameError` is thrown indicating that the variable is undefined.

## Edge Cases
1. **Local Variable Not Found**:
   - If the variable `name` is not present in both the `cells` and `vars` maps, the function will check the parent scope.
   - If the variable is still not found in the parent scope, a `NameError` is thrown.

2. **Parent Scope Not Defined**:
   - If there is no parent scope (`parent` is `nullptr`), and the variable is not found in the local scope, a `NameError` is thrown.

3. **Variable in Multiple Scopes**:
   - If the variable exists in multiple scopes (both local and parent), the value from the local scope will be returned.
   - This ensures that the most recent definition of the variable takes precedence.

## Interactions with Other Components
- **Scope Management**: The `get` function interacts with the scope management system of the Quantum Language compiler. It uses two maps (`cells` and `vars`) to store variables in the current scope.
- **Recursive Search**: If the variable is not found in the local scope, the function calls itself on the parent scope (`parent->get(name)`). This recursive search mechanism allows the compiler to access variables defined in outer scopes.
- **Exception Handling**: The function includes exception handling to manage cases where the variable is not defined. When a `NameError` is thrown, it indicates a problem with the variable's declaration or usage in the code.

## Implementation Details
Here is a detailed breakdown of how the `get` function operates:

1. **Check Local Variables**:
   - The function first attempts to find the variable `name` in the `cells` map.
   - If found, it dereferences the pointer stored in `cells[name]` and returns the value pointed to.
   - If not found in `cells`, it then checks the `vars` map for the variable `name`.

2. **Return Value**:
   - If the variable is found in either `cells` or `vars`, its value is returned directly.
   - If the variable is found in the parent scope, the value from the parent scope is returned.

3. **Throw Exception**:
   - If the variable is not found in any of the above locations, including the parent scope, a `NameError` exception is thrown.
   - The exception message includes the name of the undefined variable, providing clear feedback to the developer about the issue.

This implementation ensures that the `get` function can effectively retrieve variable values from the current scope and propagate the search up the scope chain if necessary, while also managing cases where variables are not declared properly.