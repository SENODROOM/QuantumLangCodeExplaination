# `get` Function

## Overview
The `get` function is a critical method in the Quantum Language compiler designed to retrieve the value of a variable identified by its name (`name`). This function ensures that variables can be accessed within their defined lexical scope and efficiently traverses up the scope chain if the variable is not found in the current scope.

## Parameters
- `name`: A string representing the name of the variable whose value needs to be retrieved.

## Return Value
- Returns the value associated with the variable `name`.
- If the variable is not found in the current scope or any of its parent scopes, an exception of type `NameError` is thrown with an appropriate error message indicating that the variable is undefined.

## Edge Cases
1. **Local Variable**: The function first checks if the variable exists in the local scope (`cells`). If it does, it returns the value stored in `cells`. This allows for quick access to variables that have been modified using the `&` operator.
2. **Global Variable**: If the variable is not found in the local scope, the function then checks the global scope (`vars`). If the variable is found here, it returns its value.
3. **Parent Scope**: If the variable is still not found after checking both the local and global scopes, the function recursively calls itself on the parent scope (`parent->get(name)`). This continues until the variable is found or all parent scopes have been checked.
4. **Undefined Variable**: If the variable is not found anywhere in the scope chain, the function throws a `NameError` exception. This prevents runtime errors due to accessing undefined variables.

## Interactions with Other Components
- **Scope Management**: The `get` function interacts with the scope management system, which maintains separate maps for local (`cells`) and global (`vars`) variables. These maps allow for efficient lookup of variable values based on their names.
- **Recursive Lookup**: When the variable is not found in the current scope, the function uses recursion to call itself on the parent scope. This interaction is essential for handling nested scopes where variables might be defined in outer scopes but accessed in inner scopes.
- **Exception Handling**: The `get` function utilizes exception handling to manage cases where a variable is accessed before being defined. By throwing a `NameError`, it helps catch such errors early during compilation, ensuring robustness of the code.

## Why It Works This Way
- **Efficient Access**: By checking local and global scopes first, the function minimizes the number of recursive calls, making it more efficient.
- **Lexical Scoping**: The function adheres to lexical scoping rules, allowing variables to be accessed only within their defined scope or any enclosing scope.
- **Error Prevention**: Throwing exceptions when accessing undefined variables helps prevent runtime errors and encourages developers to define variables properly before use.

This implementation ensures that the Quantum Language compiler can handle variable accesses correctly across different scopes while maintaining performance and preventing common programming errors.