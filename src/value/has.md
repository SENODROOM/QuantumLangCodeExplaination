# `has` Function

## Overview

The `has` function is a member method of the `Value` class in the Quantum Language compiler, defined in the source file `src/Value.cpp`. This function checks if a variable named `name` exists within the current scope or any of its parent scopes.

## What It Does

The `has` function returns a boolean indicating whether the variable `name` is present in the current scope. If the variable is found, it returns `true`; otherwise, it recursively checks the parent scope(s) until either the variable is found or all parent scopes have been checked, at which point it returns `false`.

### Why It Works This Way

This implementation ensures that the function correctly identifies the presence of a variable across multiple nested scopes. By checking the current scope first and then proceeding to parent scopes, it follows the typical scope resolution rules used in many programming languages, where variables declared in outer scopes are accessible to inner scopes unless shadowed by a declaration in the same scope.

## Parameters/Return Value

- **Parameters**:
  - `name`: A string representing the name of the variable to check for existence.

- **Return Value**:
  - Returns a boolean (`true` or `false`) indicating whether the variable `name` is present in the current scope or any of its parent scopes.

## Edge Cases

1. **Empty Scope**: If the current scope does not contain any variables and there are no parent scopes, the function will return `false`.
2. **Variable Shadowing**: If a variable with the same name exists in both the current scope and one of its parent scopes, the function will return `true` based on the most recent declaration encountered during the search.

## Interactions With Other Components

The `has` function interacts with the `Scope` component, as it relies on the `vars` map within the `Scope` class to store and retrieve variable names. Additionally, it uses recursion to interact with the `parent` pointer, allowing it to traverse up the scope hierarchy when necessary.

Here is an example of how you might use the `has` function:

```cpp
#include "Value.h"

int main() {
    // Create a new scope
    Scope* currentScope = new Scope();

    // Add some variables to the current scope
    currentScope->addVar("x", 5);
    currentScope->addVar("y", 10);

    // Create a parent scope and add a variable
    Scope* parentScope = new Scope();
    parentScope->addVar("z", 15);
    currentScope->setParent(parentScope);

    // Check if variables exist
    bool xExists = currentScope->getValue("x")->has("x"); // Should return true
    bool zExists = currentScope->getValue("x")->has("z"); // Should return true
    bool wExists = currentScope->getValue("x")->has("w"); // Should return false

    return 0;
}
```

In this example, the `has` function is used to verify the existence of variables `x`, `z`, and `w` within the `currentScope`. The function correctly identifies that `x` and `z` exist, while `w` does not.