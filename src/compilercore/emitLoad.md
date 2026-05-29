# `emitLoad` Function

## Overview

The `emitLoad` function is a critical component of the Quantum Language Compiler, located in the `CompilerCore.cpp` file. Its main responsibility is to load variables or constants into the current stack frame during the compilation process. This function ensures that the correct operations are emitted based on whether the target is a local variable, an upvalue, or a global variable.

### Parameters
- **const std::string &name**: The name of the variable or constant to be loaded.
- **int line**: The line number where the operation occurs, used for debugging purposes.

### Return Value
This function does not explicitly return a value; instead, it emits bytecode instructions directly using the `emit` function.

### Edge Cases
- If the variable name is `"this"`, it is treated as `"self"` because `"this"` is an alias for the first slot in all methods.
- If the variable cannot be found as a local or upvalue, it is assumed to be a global variable.

### Interactions with Other Components
- **resolveLocal**: This helper function checks if the variable is a local variable and returns its index if found. If not found, it returns `-1`.
- **resolveUpvalue**: This helper function checks if the variable is an upvalue (a variable from an enclosing scope) and returns its index if found. If not found, it returns `-1`.
- **addStr**: This function adds the string representation of the variable name to the string pool and returns its index.
- **emit**: This function is responsible for emitting the actual bytecode instructions. It takes the operation type (`Op::LOAD_LOCAL`, `Op::LOAD_UPVALUE`, `Op::LOAD_GLOBAL`), the operand (the index of the local/upvalue/global variable), and the line number as arguments.

### Why It Works This Way
The `emitLoad` function works by first determining the scope of the variable being referenced. It starts by checking if the variable is a local variable using `resolveLocal`. If successful, it emits an `OP_LOAD_LOCAL` instruction followed by the index of the local variable. If the variable is not found locally, it then checks if it is an upvalue using `resolveUpvalue`. If successful, it emits an `OP_LOAD_UPVALUE` instruction followed by the index of the upvalue. If neither a local nor an upvalue is found, it assumes the variable is global and emits an `OP_LOAD_GLOBAL` instruction followed by the index of the global variable. This approach ensures efficient loading of variables depending on their scope, optimizing both performance and memory usage during the compilation process.