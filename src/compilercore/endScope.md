# `endScope` Function

## Purpose
The `endScope` function is designed to handle the end of a scope in the quantum language compiler's code generation process. It decrements the scope depth and cleans up any local variables that are no longer needed within the current scope.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: None

## How It Works
1. **Decrement Scope Depth**:
   ```cpp
   current_->scopeDepth--;
   ```
   The scope depth is decremented by one to indicate the end of the current scope.

2. **Clean Up Local Variables**:
   ```cpp
   while (!current_->locals.empty() &&
          current_->locals.back().depth > current_->scopeDepth)
   {
       // Code to clean up locals
   }
   ```
   A loop is entered that continues as long as there are local variables on the stack (`!current_->locals.empty()`) and the depth of the last variable is greater than the new scope depth (`current_->locals.back().depth > current_->scopeDepth`). This ensures that only local variables that were declared within the current scope are removed.

3. **Emit Operations Based on Variable Capture Status**:
   Inside the loop, an operation is emitted based on whether the local variable was captured or not:
   - If the variable was captured (`current_->locals.back().isCaptured`), an `Op::CLOSE_UPVALUE` operation is emitted. This operation closes an upvalue associated with the variable, ensuring proper cleanup.
   - If the variable was not captured, an `Op::POP` operation is emitted. This operation simply pops the variable off the stack, releasing its resources.

4. **Remove Cleaned-up Variables**:
   After emitting the appropriate operation, the local variable is removed from the stack using `current_->locals.pop_back()`.

## Edge Cases
- **Empty Stack**: If the stack of local variables (`current_->locals`) is empty at the start of the function, the loop will not execute, and the function will exit immediately without performing any operations.
- **Scope Depth Mismatch**: If the depth of the last local variable is not greater than the new scope depth, the loop condition will fail, and no variables will be cleaned up.

## Interactions with Other Components
- **Code Generation**: The `emit` function is used to generate bytecode instructions. This function interacts with the compiler's intermediate representation (IR) and the output assembly code.
- **Local Variable Management**: The `current_->locals` vector manages the local variables for the current scope. As variables go out of scope, they are popped from this vector.
- **Upvalues**: If a variable is marked as captured (`isCaptured`), the `Op::CLOSE_UPVALUE` operation is emitted, which involves managing upvalues. This interaction occurs with the compiler's upvalue handling logic, which may involve additional data structures and operations.

Overall, the `endScope` function plays a crucial role in managing the lifecycle of local variables and upvalues within scopes, ensuring that resources are properly released when a scope ends.