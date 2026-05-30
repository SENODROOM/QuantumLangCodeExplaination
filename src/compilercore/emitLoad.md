# `emitLoad` Function

## Overview

The `emitLoad` function is a crucial component of the Quantum Language Compiler, located in the `CompilerCore.cpp` file. Its primary role is to load variables or constants into the current stack frame during the compilation process. This function ensures that the correct variable or constant is loaded based on its scope and type.

## Parameters

- `const std::string &name`: The name of the variable or constant to be loaded.
- `int line`: The line number where the operation occurs, used for error reporting and debugging purposes.

## Return Value

This function does not explicitly return a value. Instead, it emits appropriate bytecode instructions to load the specified variable or constant into the stack frame.

## How It Works

The `emitLoad` function operates by determining whether the variable or constant to be loaded is local, upvalue, or global within the current scope. Here's how it works:

1. **Check for Local Variable**:
   - If the name is `"this"`, it is treated as an alias for `"self"` (which is slot 0 in all methods).
   - The function then calls `resolveLocal` to check if the variable is a local variable in the current chunk (`current_`). If found, it emits an `Op::LOAD_LOCAL` instruction followed by the local index and the line number.

2. **Check for Upvalue**:
   - If the variable is not found locally, the function calls `resolveUpvalue` to check if the variable is an upvalue (a variable from an enclosing function). If found, it emits an `Op::LOAD_UPVALUE` instruction followed by the upvalue index and the line number.

3. **Load Global Variable**:
   - If the variable is neither a local nor an upvalue, it is considered a global variable. The function adds the string representation of the global name using `addStr` and emits an `Op::LOAD_GLOBAL` instruction followed by the global index and the line number.

## Edge Cases

- **Local Variable**: If the variable is a local but not found in the current chunk, it might indicate a bug in the scope resolution logic.
- **Upvalue**: If the variable is an upvalue but not found in the enclosing functions, it might indicate a bug in the upvalue resolution logic.
- **Global Variable**: If the variable is not found among locals and upvalues, it must be a global. However, if the global variable does not exist, it will cause an error when trying to load it.

## Interactions with Other Components

- **Scope Resolution**: The `emitLoad` function interacts closely with the scope resolution mechanisms (`resolveLocal`, `resolveUpvalue`) to determine the correct location of the variable or constant.
- **Bytecode Emission**: It uses the `emit` function to generate bytecode instructions (`Op::LOAD_LOCAL`, `Op::LOAD_UPVALUE`, `Op::LOAD_GLOBAL`) which are essential for the execution of the compiled code.
- **Error Handling**: By checking for the existence of variables at different scopes, `emitLoad` helps in identifying potential errors such as undefined variables.

In summary, the `emitLoad` function is vital for loading variables and constants correctly into the stack frame during the compilation process. It leverages scope resolution and bytecode emission to ensure efficient and accurate loading operations.