# resolveUpvalue

The `resolveUpvalue` function is an essential component of the Quantum Language compiler's symbol table resolution mechanism. This function plays a pivotal role in capturing variables from outer scopes and making them accessible within nested functions or closures.

## What It Does

The primary purpose of `resolveUpvalue` is to search for a variable named `name` in the current scope and its enclosing scopes. If found, it determines whether the variable is a local or an upvalue and returns the appropriate index. An upvalue is a reference to a local variable in an enclosing function that needs to be captured by a nested function or closure.

## Why It Works This Way

The function operates recursively through the scope chain, starting from the current scope (`state`). If the variable is not found in the current scope, it calls itself on the enclosing scope (`state->enclosing`). This process continues until either the variable is found or the top-level scope is reached.

If the variable is found as a local in the enclosing scope, it marks the local as captured and adds an upvalue to the current scope. The upvalue includes the index of the local variable in the enclosing scope and a boolean flag indicating that it is a local capture.

If the variable is already an upvalue in the enclosing scope, it simply adds another upvalue to the current scope without marking the original one as captured again.

This design ensures that each variable is captured only once while allowing nested functions to access variables from their enclosing scopes.

## Parameters/Return Value

- **Parameters**:
  - `state`: A pointer to the current compilation state, which contains information about the current scope and its enclosing scopes.
  - `name`: A string representing the name of the variable to be resolved.

- **Return Value**:
  - Returns the index of the upvalue if the variable is successfully resolved.
  - Returns `-1` if the variable is not found in any of the enclosing scopes.

## Edge Cases

1. **Variable Not Found**: If the variable is not present in any of the enclosing scopes, the function returns `-1`. This indicates that the variable cannot be accessed from the current scope or any nested scopes.

2. **Top-Level Scope**: When the function reaches the top-level scope (i.e., `state->enclosing` is `nullptr`), it stops searching and returns `-1`.

3. **Multiple Captures**: If a variable is captured multiple times (e.g., by different nested functions), each capture will result in a separate upvalue being added to the current scope. However, the original local variable will only be marked as captured once.

## Interactions With Other Components

- **Symbol Table**: The function interacts with the symbol table to search for the variable. The symbol table stores information about all variables declared in the current scope and its enclosing scopes.

- **Compilation State**: The function uses the compilation state to keep track of the current scope and its enclosing scopes. The compilation state also manages the list of upvalues for the current scope.

- **addUpvalue Function**: When a variable is found, the function calls `addUpvalue` to add an upvalue to the current scope. The `addUpvalue` function takes care of allocating space for the upvalue and updating the upvalue list accordingly.

Overall, the `resolveUpvalue` function is a crucial part of the Quantum Language compiler's symbol table resolution mechanism, enabling nested functions and closures to access variables from their enclosing scopes efficiently.