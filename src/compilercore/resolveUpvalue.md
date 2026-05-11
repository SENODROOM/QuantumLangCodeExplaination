# resolveUpvalue

## Purpose

The `resolveUpvalue` function is essential to the management of upvalues within nested functions in the Quantum Language compiler. Upvalues are variables that are accessible in an inner function but defined in an outer function. This function helps in resolving whether a given variable name refers to a local variable or an upvalue in the current scope and recursively searches through enclosing scopes if necessary.

## Parameters

- `state`: A pointer to the `CompilerState` structure representing the current compilation state. It contains information about the current scope and its enclosing scopes.
- `name`: A string representing the name of the variable to be resolved.

## Return Value

- Returns the index of the resolved upvalue if found.
- Returns `-1` if the variable is not found as either a local or an upvalue in any of the enclosing scopes.

## Functionality

The function operates as follows:

1. **Check Enclosing Scope**: 
   - If there is no enclosing scope (`!state->enclosing`), the function returns `-1`. This indicates that the variable is not accessible in any outer scope, and thus, cannot be an upvalue.

2. **Resolve Local Variable**:
   - The function attempts to resolve the variable name as a local variable in the enclosing scope using `resolveLocal`.
   - If the variable is found as a local (`local != -1`), it marks the local as captured by setting `state->enclosing->locals[local].isCaptured = true`.
   - It then adds the upvalue to the current scope using `addUpvalue`, passing `true` as the third argument to indicate that the upvalue is a capture of a local variable. The function returns the index of the added upvalue.

3. **Resolve Upvalue**:
   - If the variable is not found as a local, the function recursively calls itself to search for the variable in the next enclosing scope (`upvalue = resolveUpvalue(state->enclosing, name)`).
   - If the variable is found as an upvalue (`upvalue != -1`), it adds the upvalue to the current scope using `addUpvalue`, passing `false` as the third argument to indicate that the upvalue is a reference to another upvalue. The function returns the index of the added upvalue.

4. **Variable Not Found**:
   - If the variable is not found in any enclosing scope, the function returns `-1`.

## Edge Cases

- **No Enclosing Scope**: When the function is called without an enclosing scope, it correctly returns `-1` indicating that the variable cannot be an upvalue.
- **Variable Already Captured**: If a variable is already captured in an outer scope, calling `resolveUpvalue` will mark it as captured again and add a new upvalue to the current scope.
- **Variable Not Defined**: If the variable is not defined anywhere in the enclosing scopes, the function returns `-1`.

## Interactions with Other Components

- **CompilerState**: The function uses the `CompilerState` structure to keep track of the current scope and its enclosing scopes. It modifies the `locals` array in the enclosing scope to mark variables as captured.
- **resolveLocal**: This helper function is used to resolve a variable name as a local variable within the given scope. It checks the local variables table and returns their index if found.
- **addUpvalue**: This helper function adds a new upvalue to the current scope. It updates the upvalues table with the resolved index and whether the upvalue is capturing a local variable or referencing another upvalue.

In summary, the `resolveUpvalue` function plays a vital role in managing upvalues within nested functions by searching for the variable in the current and enclosing scopes, marking locals as captured when necessary, and adding upvalues to the current scope. This ensures that all variables accessed within nested functions are properly managed and compiled.