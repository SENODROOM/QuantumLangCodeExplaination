# `emitContinue`

## Overview

The `emitContinue` function is an essential component of the Quantum Language compiler's core, found within the `src/compiler/CompilerCore.cpp` file. This function is responsible for generating a jump instruction that enables the program to resume execution at the beginning of the nearest enclosing loop.

### Why It Works This Way

In quantum programming, as in traditional programming, loops play a crucial role in controlling the flow of execution and repeating tasks. When a loop encounters a `continue` statement, it should skip the current iteration and proceed directly to the next one. The `emitContinue` function facilitates this behavior by emitting a jump instruction that directs control back to the loop's entry point.

This approach ensures that the loop remains intact and continues executing subsequent iterations without being broken or altered, thereby maintaining the intended flow and logic of the program.

## Parameters/Return Value

- **Parameters**:
  - None

- **Return Value**:
  - Returns nothing (`void`)

## Edge Cases

1. **No Enclosing Loop**: If there is no enclosing loop when a `continue` statement is encountered, the `emitContinue` function will not generate any jump instructions. Instead, it will simply ignore the `continue` statement, as there is nowhere to jump to.

2. **Nested Loops**: In scenarios where multiple nested loops exist, the `emitContinue` function will only affect the nearest enclosing loop. It will not cause a jump out of all nested loops simultaneously.

3. **Empty Loop Body**: If the body of a loop is empty (i.e., contains no statements), encountering a `continue` statement within that loop will result in a jump to the loop's end, effectively skipping the entire loop.

## Interactions With Other Components

The `emitContinue` function interacts closely with the `loops_` stack, which keeps track of the current state of all active loops during compilation. Each time a new loop is entered, a new loop context is pushed onto the stack. The `emitContinue` function then appends the generated jump instruction to the `continueJumps` vector of the topmost loop context on the stack.

When the compiler encounters a `continue` statement, it calls the `emitContinue` function to generate the necessary jump instruction. This instruction is stored in the `continueJumps` vector and will be executed later when the loop's condition is checked again, allowing the loop to skip the current iteration and proceed to the next one.

Upon exiting a loop, whether through normal completion, a `break` statement, or an exception, the compiler pops the corresponding loop context from the `loops_` stack. This ensures that the `continueJumps` vector associated with the exited loop is no longer accessible, preventing potential misuse or unintended jumps into already exited loops.