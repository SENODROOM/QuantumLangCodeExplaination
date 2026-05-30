# resolveUpvalue

## Purpose

The `resolveUpvalue` function is crucial for managing upvalues within nested functions in the Quantum Language compiler. An upvalue is a variable that is accessible in an inner function but defined in an outer function. This function ensures that upvalues are correctly captured and resolved during the compilation process.

## Functionality

When called, `resolveUpvalue` performs the following steps:

1. **Check Enclosing Scope**: The function first checks if there is an enclosing scope (`state->enclosing`). If not, it returns `-1`, indicating that the variable is not found in any accessible scope.

2. **Resolve Local Variable**: It attempts to resolve the variable as a local variable in the enclosing scope using the `resolveLocal` function. If successful, it marks the local variable as captured (`state->enclosing->locals[local].isCaptured = true`) and then adds the upvalue to the current function's upvalue list using the `addUpvalue` function with `true` as the second argument, indicating that it is a local upvalue.

3. **Resolve Upvalue**: If the variable is not found as a local variable in the enclosing scope, it recursively calls `resolveUpvalue` on the parent scope (`state->enclosing->enclosing`). If the variable is found as an upvalue in the parent scope, it adds the upvalue to the current function's upvalue list using the `addUpvalue` function with `false` as the second argument, indicating that it is a non-local upvalue.

4. **Return Value**: If the variable is successfully resolved, whether as a local or non-local upvalue, the function returns the index of the upvalue in the current function's upvalue list. If the variable is not found in any accessible scope, it returns `-1`.

## Parameters/Return Value

- **Parameters**:
  - `state`: A pointer to the current compilation state, which includes information about the current function and its enclosing scopes.
  - `name`: A string representing the name of the variable being resolved.

- **Return Value**:
  - An integer representing the index of the resolved upvalue in the current function's upvalue list. Returns `-1` if the variable is not found in any accessible scope.

## Edge Cases

- **No Enclosing Scope**: If there is no enclosing scope (`state->enclosing` is `nullptr`), the function will return `-1`.
- **Variable Not Found**: If the variable is not found as either a local variable or an upvalue in any accessible scope, the function will return `-1`.

## Interactions with Other Components

- **Enclosing Scopes**: `resolveUpvalue` interacts with the enclosing scopes to find the variable. Each scope has a list of locals and upvalues, and the function searches through these lists.
- **State Management**: The function uses the compilation state (`state`) to keep track of the current function and its enclosing scopes. This allows it to navigate through the scope hierarchy and resolve variables accordingly.
- **Upvalue List**: `resolveUpvalue` updates the upvalue list of the current function when it resolves a new upvalue. This list is used during code generation to manage how upvalues are accessed and passed between functions.

In summary, `resolveUpvalue` is a vital component for handling upvalues in nested functions within the Quantum Language compiler. By ensuring that upvalues are correctly captured and resolved, it supports the dynamic scoping mechanism required by the language, enabling variables defined in outer functions to be accessed by inner functions.