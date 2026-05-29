# `endScope` Function

## Purpose
The `endScope` function is crucial for managing the end of a scope within the Quantum Language compiler. It primarily decrements the scope depth and handles the cleanup of local variables that have fallen out of scope. This ensures that resources are properly released and upvalues are closed when necessary.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: Void

## How It Works
1. **Decrement Scope Depth**: The function first decrements the `scopeDepth` member variable of the current compilation context (`current_`). This step indicates that one level of scope has ended.

2. **Cleanup Local Variables**:
   - A `while` loop checks if there are any local variables in the `locals` stack that belong to the now-ended scope.
   - For each local variable, it determines whether the variable is captured or not using the `isCaptured` flag.
     - If the variable is captured, it emits an `Op::CLOSE_UPVALUE` operation. Upvalues are special references to variables in outer scopes that are used in closures. Closing an upvalue means that the reference is no longer needed and can be safely removed.
     - If the variable is not captured, it emits an `Op::POP` operation. This operation removes the variable from the stack, freeing up its memory.
   - After handling the variable, it pops the variable from the `locals` stack.

3. **Edge Cases**:
   - **No Locals**: If there are no local variables in the `locals` stack, the function simply returns without doing anything.
   - **Nested Scopes**: When dealing with nested scopes, the function only cleans up variables that belong to the immediately ended scope. Variables in outer scopes remain intact until their respective scopes end.

## Interactions with Other Components
- **Compilation Context (`current_`)**: The function operates on the `current_` compilation context, which holds information about the current state of the compilation process, including the scope depth and the list of local variables.
- **Emitting Operations**: The function uses the `emit` method to generate bytecode operations. These operations include `Op::CLOSE_UPVALUE` and `Op::POP`, which are responsible for closing upvalues and popping variables from the stack, respectively.
- **Local Variable Management**: The function manages the `locals` stack, which stores information about local variables. Each local variable includes its depth and whether it is captured.

By following these steps, the `endScope` function ensures that the Quantum Language compiler correctly handles the end of a scope, releasing resources and maintaining the integrity of the compiled code.