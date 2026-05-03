# `emitLoad` Function

## Overview

The `emitLoad` function is a critical method within the Quantum Language Compiler's `CompilerCore.cpp` file. Its primary purpose is to load a variable or constant into the current stack frame during the compilation process. This function interacts closely with the symbol table and the bytecode generation mechanism to ensure that variables and constants are correctly accessed and loaded onto the stack.

## Parameters

- `const std::string &name`: The name of the variable or constant to be loaded.
- `int line`: The source code line number where the `emitLoad` call originates. This parameter is used for error reporting and debugging purposes.

## Return Value

This function does not explicitly return any value. Instead, it generates bytecode instructions using the `emit` function based on the type of entity being loaded.

## Edge Cases

1. **Loading 'this'**: If the name to be loaded is `'this'`, it is aliased as `'self'`. This is because in the context of methods, `'this'` typically refers to the instance itself, which is represented as `'self'`.
2. **Local Variables**: If the variable is found in the current scope (i.e., it is a local variable), the function emits an `Op::LOAD_LOCAL` instruction followed by the index of the local variable.
3. **Upvalues**: If the variable is not found in the current scope but exists in an enclosing scope (i.e., it is an upvalue), the function emits an `Op::LOAD_UPVALUE` instruction followed by the index of the upvalue.
4. **Global Variables**: If the variable is neither a local nor an upvalue, it must be a global variable. In this case, the function emits an `Op::LOAD_GLOBAL` instruction followed by the string representation of the global variable name.

## Interactions with Other Components

- **Symbol Table**: The `resolveLocal` and `resolveUpvalue` functions rely on the symbol table to determine whether the variable is a local or upvalue. The symbol table keeps track of the names and types of variables in different scopes.
- **Bytecode Generation**: The `emit` function is responsible for generating the actual bytecode instructions. These instructions are then executed by the quantum virtual machine at runtime to manage the stack and access variables.

## Detailed Explanation

Here is a step-by-step breakdown of how the `emitLoad` function operates:

1. **Name Resolution**:
   ```cpp
   const std::string &resolved = (name == "this") ? std::string("self") : name;
   ```
   - If the input name is `'this'`, it is replaced with `'self'`. This replacement is necessary because `'this'` often represents the current object instance in method definitions.

2. **Local Variable Lookup**:
   ```cpp
   int local = resolveLocal(current_, resolved);
   if (local != -1)
   {
       emit(Op::LOAD_LOCAL, local, line);
       return;
   }
   ```
   - The `resolveLocal` function searches the current scope for the variable. If found, it returns the index of the local variable.
   - If a local variable is found, the function emits an `Op::LOAD_LOCAL` instruction with the local variable index and the source code line number.
   - The function then returns immediately after emitting the instruction.

3. **Upvalue Lookup**:
   ```cpp
   int uv = resolveUpvalue(current_, resolved);
   if (uv != -1)
   {
       emit(Op::LOAD_UPVALUE, uv, line);
       return;
   }
   ```
   - If the variable is not found as a local, the `resolveUpvalue` function searches the enclosing scopes to find the variable as an upvalue.
   - If an upvalue is found, the function emits an `Op::LOAD_UPVALUE` instruction with the upvalue index and the source code line number.
   - The function then returns immediately after emitting the instruction.

4. **Global Variable Lookup**:
   ```cpp
   emit(Op::LOAD_GLOBAL, addStr(resolved), line);
   ```
   - If the variable is neither a local nor an upvalue, it is considered a global variable.
   - The `addStr` function adds the global variable name to the string pool and returns its index.
   - The function then emits an `Op::LOAD_GLOBAL` instruction with the index of the global variable and the source code line number.

By following these steps, the `emitLoad` function ensures that the correct bytecode instructions are generated to load variables and constants into the stack frame, facilitating efficient execution of the compiled quantum programs.