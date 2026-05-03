# `get` Function

## Overview
The `get` function is a critical method within the Quantum Language compiler used to retrieve the value of a variable specified by its name (`name`). This function facilitates access to variables within the current lexical scope and seamlessly navigates up the scope hierarchy to resolve the variable if not found in the local context.

### Parameters
- `name`: A string representing the name of the variable whose value needs to be retrieved.

### Return Value
- Returns the value associated with the variable named `name`.
- If the variable is not found in the current or any parent scope, throws a `NameError`.

### Edge Cases
- **Local Variable**: If the variable exists in the current scope (`cells`), the function returns its value directly.
- **Parent Scope**: If the variable is not found in the current scope but exists in a parent scope, the function recursively calls itself on the parent scope until the variable is found or all scopes have been checked.
- **Undefined Variable**: If the variable is not found in any scope, the function throws a `NameError`, indicating that the variable is undefined.

### Interactions with Other Components
- **Scope Management**: The `get` function interacts with the scope management system, which maintains both local (`cells`) and global (`vars`) variable storage.
- **Recursive Lookup**: When a variable is not found in the current scope, the function uses recursion to call itself on the parent scope(s). This ensures that nested scopes can be accessed correctly.
- **Error Handling**: The function includes error handling to manage cases where a variable is not defined. It throws a `NameError` when an undefined variable is accessed, which can be caught and handled by higher-level code.

### Why It Works This Way
The design of the `get` function allows for efficient variable lookup by prioritizing local variables over parent variables. By checking both `cells` and `vars` maps, the function ensures that the most recently assigned values are retrieved. The recursive approach allows the function to handle nested scopes gracefully, making it versatile for compilers managing complex program structures. The inclusion of error handling ensures robustness, preventing runtime errors due to undefined variable references.