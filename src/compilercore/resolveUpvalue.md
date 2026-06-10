# resolveUpvalue

## Purpose

The `resolveUpvalue` function plays a vital role in managing upvalues within nested functions in the Quantum Language compiler. An upvalue is a variable that is accessible in an inner function but defined in an outer function. This function ensures that upvalues are correctly resolved and added to the current function's list of upvalues.

## Parameters

- `State* state`: A pointer to the current compilation state, which includes information about the enclosing function and the list of locals.
- `const std::string& name`: The name of the variable whose upvalue needs to be resolved.

## Return Value

- `int`: Returns the index of the upvalue if successfully resolved; otherwise, returns `-1`.

## How It Works

The `resolveUpvalue` function operates as follows:

1. **Check Enclosing Function**: If there is no enclosing function (`!state->enclosing`), it immediately returns `-1`. This indicates that the variable is not an upvalue since there is no outer scope to capture variables from.

2. **Resolve Local Variable**: It attempts to resolve the variable locally in the enclosing function using the `resolveLocal` function. If the variable is found (`local != -1`), it sets the `isCaptured` flag of the local variable to `true`, indicating that it is being captured by the inner function. Then, it adds the local variable as an upvalue to the current function's list using the `addUpvalue` function with `isLocal` set to `true`.

3. **Resolve Upvalue**: If the variable is not found locally, it recursively calls itself on the enclosing function to check if the variable is an upvalue of the enclosing function (`upvalue != -1`). If it is found, it adds the upvalue to the current function's list using the `addUpvalue` function with `isLocal` set to `false`.

4. **Return Result**: Regardless of whether the variable is found locally or as an upvalue, the function returns the index of the upvalue. If the variable is not found at all, it returns `-1`.

## Edge Cases

- **No Enclosing Function**: If the function is called without an enclosing function, it will always return `-1`.
- **Variable Not Found**: If the variable is neither found locally nor as an upvalue in any enclosing function, the function will also return `-1`.

## Interactions with Other Components

- **resolveLocal**: This function is used to find the local variable in the enclosing function. If the variable is found, its index is returned.
- **addUpvalue**: This function is used to add the resolved upvalue to the current function's list of upvalues. It takes the index of the upvalue and a boolean indicating whether the upvalue is local to the enclosing function.

By interacting with these components, `resolveUpvalue` ensures that upvalues are correctly managed throughout the nested function structure, allowing inner functions to access variables defined in their outer functions.