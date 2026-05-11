# `get` Function

## Overview
The `get` function is a crucial method within the Quantum Language compiler that retrieves the value of a variable specified by its name (`name`). This function facilitates access to variables within the current lexical scope and seamlessly navigates up the scope hierarchy if the variable is not found in the local scope.

### Parameters
- **name**: A string representing the name of the variable whose value needs to be retrieved.

### Return Value
- Returns the value associated with the variable named `name`.
- If the variable is found in either the local `cells` or `vars` map, the corresponding value is returned.
- If the variable is not found in the local scope but exists in an enclosing scope, the function recursively calls itself on the parent scope until the variable is found.
- If the variable is undefined in all scopes, a `NameError` exception is thrown indicating that the variable is not defined.

### Edge Cases
1. **Local Variable**: The variable might exist only in the local scope (`cells` or `vars`). In such cases, the function returns the value directly from these maps.
2. **Enclosing Scope**: If the variable is not found in the local scope but exists in an enclosing scope, the function correctly traverses up the scope hierarchy using recursion.
3. **Undefined Variable**: If the variable is completely undefined across all scopes, the function raises a `NameError`, ensuring that any attempt to use an undefined variable results in an error.

### Interactions with Other Components
- **Scope Management**: The `get` function interacts with the scope management system, which maintains two main maps:
  - `cells`: Stores pointers to live values of variables that have been modified using the `&` operator.
  - `vars`: Stores the actual values of variables.
- **Recursion**: When the variable is not found in the local scope, the function uses recursion to call itself on the parent scope. This interaction ensures that the function can access variables defined in higher-level scopes.
- **Exception Handling**: If the variable is undefined, the function throws a `NameError`. This interaction with the exception handling mechanism helps in identifying and reporting errors related to undefined variables.

### Example Usage
Here's an example demonstrating how the `get` function might be used:

```cpp
#include "Value.h"

int main() {
    // Create a new scope
    Scope* currentScope = new Scope();

    // Define a variable in the current scope
    currentScope->vars["x"] = 42;

    // Retrieve the value of 'x'
    int xValue = currentScope->get("x");
    std::cout << "Value of x: " << xValue << std::endl; // Output: Value of x: 42

    // Clean up
    delete currentScope;

    return 0;
}
```

In this example, the `get` function successfully retrieves the value of the variable `x` from the current scope. If the variable were not defined in the current scope but existed in an enclosing scope, the function would traverse up the scope hierarchy to find and return the value. If the variable were completely undefined, the function would raise a `NameError`.

By understanding the behavior and implementation details of the `get` function, developers can effectively manage variable access within their quantum programs, ensuring that variables are correctly referenced and updated throughout the execution process.