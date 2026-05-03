# resolveUpvalue

The `resolveUpvalue` function is a crucial component of the Quantum Language compiler's core functionality, specifically responsible for managing upvalues within nested functions. An upvalue is a variable that is accessible in an inner function but defined in an outer function.

## Functionality

The primary role of `resolveUpvalue` is to locate and manage upvalues. It first checks if there is an enclosing function (`state->enclosing`). If not, it returns `-1`, indicating that the upvalue cannot be resolved. If an enclosing function exists, it attempts to resolve the variable as a local variable in that enclosing function using the `resolveLocal` method. 

If the variable is found as a local in the enclosing function, it marks that local as captured (`state->enclosing->locals[local].isCaptured = true`) and then adds an upvalue to the current function's closure using the `addUpvalue` method. The `addUpvalue` method takes two arguments: the index of the local variable and a boolean flag indicating whether the upvalue is a local or an upvalue itself.

If the variable is not found as a local in the enclosing function, `resolveUpvalue` recursively calls itself on the enclosing function to attempt to resolve the variable as an upvalue. If successful, it adds another upvalue to the current function's closure using the `addUpvalue` method without marking the original local as captured.

Finally, if the variable cannot be resolved either as a local or an upvalue in any enclosing function, `resolveUpvalue` returns `-1`.

## Parameters and Return Value

- **Parameters**:
  - `state`: A pointer to the compilation state, which includes information about the current function and its enclosing functions.
  - `name`: A string representing the name of the variable to be resolved as an upvalue.

- **Return Value**:
  - Returns the index of the upvalue in the current function's closure if the variable is successfully resolved as an upvalue.
  - Returns `-1` if the variable cannot be resolved as either a local or an upvalue in any enclosing function.

## Edge Cases

- **No Enclosing Function**: If the current function has no enclosing function (`state->enclosing == nullptr`), `resolveUpvalue` immediately returns `-1`.
- **Variable Not Found**: If the variable is neither found as a local nor an upvalue in any enclosing function, `resolveUpvalue` returns `-1`.

## Interactions with Other Components

- **resolveLocal**: This helper function is used to search for the variable as a local in the enclosing function. If found, it marks the local as captured and returns its index.
- **addUpvalue**: This helper function is used to add an upvalue to the current function's closure. It takes the index of the local or upvalue and a boolean flag indicating whether it is a local or an upvalue itself.

Overall, `resolveUpvalue` plays a vital role in capturing variables from enclosing functions and managing them effectively within nested functions, ensuring proper scope and lifetime handling during compilation.