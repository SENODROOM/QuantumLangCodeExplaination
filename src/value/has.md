# `has` Function

## Overview

The `has` function is a member method of the `Value` class located in the Quantum Language compiler's source file `src/Value.cpp`. This function determines whether a variable named `name` is present within the current scope or any of its parent scopes.

## What It Does

The `has` function performs a lookup to check if a variable with the specified name (`name`) exists in the `vars` map, which represents the variables defined in the current scope. If the variable is found in the current scope, the function returns `true`.

If the variable is not found in the current scope, the function checks if there is a parent scope by examining the `parent` pointer. If a parent scope exists, it recursively calls the `has` function on the parent scope to search for the variable. If the variable is found in any parent scope, the function returns `true`.

If neither the current scope nor any parent scope contains the variable, the function returns `false`.

### Example Usage

Here's an example demonstrating how the `has` function might be used:

```cpp
#include "Value.h"

int main() {
    Value currentScope;
    currentScope.vars["x"] = 42;

    Value parentScope;
    parentScope.vars["y"] = 99;

    currentScope.parent = &parentScope;

    // Check if 'x' exists in the current scope or any parent scopes
    bool xExists = currentScope.has("x"); // Returns true
    bool yExists = currentScope.has("y"); // Returns true
    bool zExists = currentScope.has("z"); // Returns false

    return 0;
}
```

In this example:
- The variable `x` is defined in the current scope and is accessible through the `has` function.
- The variable `y` is defined in the parent scope and is also accessible through the `has` function.
- The variable `z` is not defined in either the current scope or the parent scope, so the `has` function returns `false`.

## Why It Works This Way

The `has` function works by leveraging the hierarchical nature of scopes in the Quantum Language compiler. By checking both the current scope and its parent scopes, the function ensures that all possible variable definitions are considered. This approach allows for a flexible and dynamic variable lookup mechanism, accommodating nested scopes and variable shadowing.

### Edge Cases

1. **Empty Scope**: If the current scope has no variables defined (`vars` map is empty) and there is no parent scope (`parent` pointer is `nullptr`), the function will correctly return `false`.
2. **Variable Shadowing**: If a variable with the same name is defined in both the current scope and a parent scope, the function will return `true` for the first occurrence encountered during the lookup process.

## Parameters/Return Value

### Parameters

- `const std::string& name`: A constant reference to the string representing the name of the variable to look up.

### Return Value

- `bool`: Returns `true` if the variable exists in the current scope or any of its parent scopes; otherwise, returns `false`.

## Interactions With Other Components

The `has` function interacts with the `Value` class's `vars` map and `parent` pointer. The `vars` map stores the variables defined in the current scope, while the `parent` pointer points to the parent scope, allowing for recursive searches when necessary.

This function is crucial for resolving variable references in the Quantum Language compiler, ensuring that the correct variable definition is accessed based on the current scope context.