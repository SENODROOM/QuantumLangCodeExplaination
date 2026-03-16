# `compileFor` Function

## Overview

The `compileFor` function in the Quantum Language compiler is responsible for compiling a `for` loop construct into bytecode. This function handles the initialization, iteration, and cleanup phases of the loop, ensuring that the loop variable(s) are properly declared, assigned, and updated during each iteration.

## Parameters and Return Value

- **Parameters**: The function takes a single parameter `s`, which is an instance of the `ForStatement` class representing the `for` loop construct to be compiled.
- **Return Value**: None. The function compiles the `for` loop and emits corresponding bytecode instructions directly into the current chunk.

## How It Works

### Initialization Phase

1. **Compile Iterable Expression**:
   ```cpp
   compileExpr(*s.iterable);
   ```
   This line compiles the expression that generates the iterable object (e.g., a list or range). The resulting bytecode will store the iterable object on the stack.

2. **Emit Iterator Creation Instruction**:
   ```cpp
   emit(Op::MAKE_ITER, 0, line);
   ```
   This instruction creates an iterator from the iterable object on the stack. The iterator is then pushed onto the stack.

3. **Begin Outer Scope**:
   ```cpp
   beginScope(); // outer scope — iterator lives here
   ```
   A new scope is started where the iterator will live. This ensures that the iterator remains accessible throughout the loop.

4. **Declare and Define Iterator Local Variable**:
   ```cpp
   declareLocal("__iter__", line);
   int iterSlot = static_cast<int>(current_->locals.size()) - 1;
   emit(Op::DEFINE_LOCAL, iterSlot, line);
   ```
   An iterator local variable named `__iter__` is declared and defined. The slot number of this local variable is stored in `iterSlot`.

### Iteration Phase

5. **Mark Loop Start**:
   ```cpp
   int loopStart = static_cast<int>(chunk().code.size());
   beginLoop(loopStart);
   ```
   The start position of the loop is recorded, and a new loop block is begun.

6. **Emit FOR_ITER Instruction**:
   ```cpp
   size_t exitJump = emitJump(Op::FOR_ITER, line);
   ```
   The `FOR_ITER` instruction is emitted. This instruction pops the iterator from the stack and attempts to fetch the next item. If the iterator is exhausted, it jumps to the location specified by `exitJump`. Otherwise, it pushes the next item onto the stack.

7. **Begin Inner Scope**:
   ```cpp
   beginScope();
   declareLocal(s.var, line);
   int varSlot = static_cast<int>(current_->locals.size()) - 1;
   emit(Op::DEFINE_LOCAL, varSlot, line);
   ```
   A new inner scope is started where the loop variable will live. The loop variable is declared and defined, and its slot number is stored in `varSlot`.

8. **Handle Second Loop Variable (if present)**:
   ```cpp
   if (!s.var2.empty())
   {
       emit(Op::LOAD_LOCAL, varSlot, line);
       emit(Op::LOAD_CONST, addConst(QuantumValue(1.0)), line);
       emit(Op::GET_INDEX, 0, line);
       declareLocal(s.var2, line);
       emit(Op::DEFINE_LOCAL, static_cast<int>(current_->locals.size()) - 1, line);
       emit(Op::LOAD_LOCAL, varSlot, line);
       emit(Op::LOAD_CONST, addConst(QuantumValue(0.0)), line);
       emit(Op::GET_INDEX, 0, line);
       emit(Op::STORE_LOCAL, varSlot, line);
       emit(Op::POP, 0, line);
   }
   ```
   If there is a second loop variable (`s.var2`), additional bytecode instructions are emitted to extract indices from the tuple returned by the iterator. These indices are then assigned to the second loop variable.

9. **Compile Loop Body**:
   ```cpp
   compileNode(*s.body);
   ```
   The body of the loop is compiled. During each iteration, the loop variable(s) are available within this scope.

### Cleanup Phase

10. **Patch Continue Jumps**:
    ```cpp
    for (size_t ci : loops_.back().continueJumps)
        chunk().patch(ci, static_cast<int32_t>(chunk().code.size()) - static_cast<int32_t>(ci) - 1);
    ```
    Any continue jumps within the loop are patched to ensure they jump correctly to the beginning of the loop.

11. **End Inner Scope**:
    ```cpp
    endScope(line);
    ```
    The inner scope is ended, popping the loop variable(s) but leaving the iterator on the stack.

12. **Emit LOOP Instruction**:
    ```cpp
    emit(Op::LOOP, static_cast<int>(chunk().code.size()) - loopStart + 1, line);
    ```
    The `LOOP` instruction is emitted, causing control to return to the `FOR_ITER` instruction after each iteration.

13. **Patch Exit Jump**:
    ```cpp
    patchJump(exitJump);
    ```
    The `exitJump` is patched to ensure it jumps to the correct location when the iterator is exhausted.

14. **End Outer Scope**:
    ```cpp
    endScope(line);
    endLoop();
    ```
    The outer scope is ended, popping the iterator, and the loop block is concluded.

## Edge Cases

- **Empty Iterable**: If the iterable expression evaluates to an empty collection, the `FOR_ITER` instruction will eventually exhaust the iterator, and the loop body will not execute.
- **Single Variable Loop**: If there