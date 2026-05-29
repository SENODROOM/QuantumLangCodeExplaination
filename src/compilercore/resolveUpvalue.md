# resolveUpvalue

## Purpose

The `resolveUpvalue` function manages upvalues within nested functions in the Quantum Language compiler. An upvalue is a variable that is accessible in an inner function but defined in an outer function. This function ensures that upvalues are correctly captured and resolved during the compilation process.

## Parameters

- `State* state`: A pointer to the current compilation state, which includes information about the enclosing function and the list of locals.
- `const std::string& name`: The name of the variable being resolved as an upvalue.

## Return Value

- `int`: The index of the resolved upvalue or `-1` if the variable cannot be found as an upvalue.

## How It Works

The function operates in a recursive manner, starting from the current function's enclosing scope and moving outward until it finds the variable or reaches the top-level scope. Here’s a step-by-step breakdown of how it works:

1. **Check Enclosing Function**: 
   - If there is no enclosing function (`!state->enclosing`), the function returns `-1`, indicating that the variable is not an upvalue.
   
2. **Resolve Local Variable**:
   - The function attempts to resolve the variable as a local in the enclosing function using `resolveLocal`.
   - If the variable is found as a local (`local != -1`), it marks the local as captured (`state->enclosing->locals[local].isCaptured = true`) and adds an upvalue to the current function’s list of upvalues using `addUpvalue`. The boolean parameter `true` indicates that the upvalue is a local capture.
   - If the variable is successfully added as an upvalue, the function returns its index.

3. **Resolve Upvalue**:
   - If the variable is not found as a local, the function recursively calls itself on the enclosing function (`resolveUpvalue(state->enclosing, name)`).
   - If the variable is found as an upvalue in the enclosing function (`upvalue != -1`), it adds the upvalue to the current function’s list of upvalues using `addUpvalue`. The boolean parameter `false` indicates that the upvalue is a non-local capture.
   - If the variable is successfully added as an upvalue, the function returns its index.

4. **Return `-1`**:
   - If the variable is not found in any enclosing function, the function returns `-1`.

## Edge Cases

- **No Enclosing Function**: If the current function has no enclosing function, the variable cannot be an upvalue, so the function returns `-1`.
- **Variable Not Found**: If the variable is not found in either the local scope or any enclosing scopes, the function returns `-1`.

## Interactions With Other Components

- **resolveLocal**: This helper function is used to find a variable as a local in the given function’s scope.
- **addUpvalue**: This helper function adds an upvalue to the current function’s list of upvalues, handling both local and non-local captures.

By leveraging these helper functions, `resolveUpvalue` ensures that all upvalues are correctly identified and managed during the compilation process, allowing nested functions to access variables from their enclosing scopes efficiently.