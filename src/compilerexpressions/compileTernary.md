# `compileTernary`

The `compileTernary` function in the Quantum Language compiler is responsible for handling the compilation of ternary expressions. Ternary expressions provide a compact way to evaluate conditions and select between two possible outcomes based on whether the condition is true or false.

## Function Overview

### Parameters

- `e`: A reference to a `TernaryExpression` object representing the ternary expression to be compiled.

### Return Value

This function does not explicitly return a value; instead, it modifies the bytecode being generated through calls to various helper functions like `emit`, `emitJump`, and `patchJump`.

### Edge Cases

1. **Empty Condition**: If the condition part of the ternary expression is empty, the function should handle it gracefully without causing errors.
2. **Null Expressions**: The function should ensure that both the `thenExpr` and `elseExpr` are non-null before attempting to compile them.
3. **Type Mismatch**: The types of `thenExpr` and `elseExpr` must match. If they do not, the function should raise an error during compilation.

## Why It Works This Way

The implementation of `compileTernary` follows a structured approach to handle the conditional logic efficiently:

1. **Compile Condition**: The first step is to compile the condition expression (`e.condition`). This evaluates the condition and prepares the stack for subsequent operations.
   
   ```cpp
   compileExpr(*e.condition);
   ```

2. **Emit Jump If False**: After compiling the condition, the function emits a jump instruction (`Op::JUMP_IF_FALSE`) that will skip the `thenExpr` block if the condition is false. The position of this jump is stored in `elseJump`, which will later be patched when the `thenExpr` block ends.

   ```cpp
   size_t elseJump = emitJump(Op::JUMP_IF_FALSE, line);
   ```

3. **Pop Condition Result**: The result of the condition expression is popped off the stack since it's no longer needed after the jump has been emitted.

   ```cpp
   emit(Op::POP, 0, line);
   ```

4. **Compile Then Expression**: Next, the `thenExpr` block is compiled. If the condition is true, execution will continue here.

   ```cpp
   compileExpr(*e.thenExpr);
   ```

5. **Emit Jump to End**: After compiling the `thenExpr` block, another jump instruction (`Op::JUMP`) is emitted to skip over the `elseExpr` block. The position of this jump is stored in `endJump`, which will also be patched later.

   ```cpp
   size_t endJump = emitJump(Op::JUMP, line);
   ```

6. **Patch Else Jump**: The `elseJump` is patched at its stored position to point to the start of the `elseExpr` block. This ensures that if the condition is false, execution will jump to the `elseExpr` block.

   ```cpp
   patchJump(elseJump);
   ```

7. **Pop Again**: The result of the `thenExpr` block is popped off the stack as it's no longer needed after the jump has been emitted.

   ```cpp
   emit(Op::POP, 0, line);
   ```

8. **Compile Else Expression**: Finally, the `elseExpr` block is compiled. If the condition is false, execution will continue here.

   ```cpp
   compileExpr(*e.elseExpr);
   ```

9. **Patch End Jump**: The `endJump` is patched at its stored position to ensure that after either the `thenExpr` or `elseExpr` block is executed, the program continues beyond the ternary expression.

   ```cpp
   patchJump(endJump);
   ```

## Interactions with Other Components

- **Bytecode Emission**: The `compileTernary` function interacts with the bytecode emission system through calls to `emit` and `emitJump`. These functions manage the generation of machine code instructions.
  
- **Jump Patching**: To ensure correct control flow, the function uses `patchJump` to update jump targets dynamically. This interaction is crucial for maintaining the integrity of the compiled program.

- **Error Handling**: The function may interact with error handling mechanisms within the compiler to report type mismatches or null expressions, ensuring robustness during compilation.

By following this structured approach, the `compileTernary` function effectively handles the compilation of ternary expressions, ensuring efficient and correct bytecode generation while maintaining the overall integrity of the quantum language program.