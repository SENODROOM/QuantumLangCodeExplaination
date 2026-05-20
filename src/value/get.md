# `get` Function

## Overview
The `get` function is a crucial method in the Quantum Language compiler designed to retrieve the value of a variable identified by its name (`name`). This function ensures that variables can be accessed within their defined lexical scope and efficiently traverses up the scope hierarchy if necessary.

### Parameters
- **name**: A string representing the name of the variable whose value needs to be retrieved.

### Return Value
- The value associated with the variable named `name`. If the variable exists in the local scope, the function returns its value directly. If the variable is not found in the local scope but exists in an upper scope, the function recursively calls itself on the parent scope until the variable is found or the topmost scope is reached. If the variable is still undefined after checking all scopes, a `NameError` exception is thrown indicating that the variable is undefined.

### Edge Cases
1. **Local Variable**: If the variable is defined locally within the current scope, the function will immediately return its value without searching any higher scopes.
2. **Parent Scope Variable**: If the variable is not found in the local scope but is defined in one of the parent scopes, the function will continue to search upwards until the variable is located.
3. **Undefined Variable**: If the variable is neither defined in the local nor any parent scope, a `NameError` is thrown, signaling that the variable is undefined.

### Interactions with Other Components
- **Scope Management**: The `get` function interacts closely with the scope management system of the compiler. It uses two maps (`cells` and `vars`) to store variables at different levels of the scope hierarchy. These maps facilitate quick lookups of variable values within their respective scopes.
- **Recursive Calls**: When the variable is not found in the current scope, the function makes recursive calls to its parent scope(s) using the `parent` pointer. This interaction allows the function to traverse up the scope chain and find the variable if it exists in a higher scope.
- **Exception Handling**: The function includes exception handling to manage cases where a variable is not defined. By throwing a `NameError`, it informs the user about the undefined variable, which helps in debugging and error resolution during compilation.

This comprehensive approach ensures that the `get` function provides efficient and accurate retrieval of variable values across different scopes, enhancing the functionality and reliability of the Quantum Language compiler.