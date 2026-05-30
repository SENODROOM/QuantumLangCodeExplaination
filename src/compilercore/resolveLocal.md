# `resolveLocal` Function

## Overview

The `resolveLocal` function is an essential utility method in the Quantum Language compiler's `CompilerCore.cpp` file. Its primary purpose is to locate and return the index of a local variable within the current lexical scope by its name. This function aids in optimizing variable access and ensuring that variables are correctly referenced during compilation.

## Parameters

- **`state`**: A pointer to the current compilation state, which contains information about the current scope and all declared variables.
- **`name`**: The name of the local variable whose index needs to be resolved.

## Return Value

- **`int`**: Returns the index of the local variable if found; otherwise, returns `-1`.

## How It Works

The function iterates through the list of local variables stored in the `state->locals` vector. It starts from the end of the vector and moves backward, checking each variable's name against the provided `name`. If a match is found, the function immediately returns the index of that variable. This reverse iteration ensures that the most recently declared variable takes precedence over any previously declared variables with the same name, adhering to the Last In First Out (LIFO) principle of stack-based scoping.

If the function completes the loop without finding a match, it returns `-1`, indicating that the variable is not present in the current scope.

## Edge Cases

- **Variable Not Found**: If the variable with the given name does not exist in the current scope, the function returns `-1`.
- **Multiple Variables with Same Name**: Although the function uses reverse iteration, it assumes that there will only be one declaration of a variable with a given name within the current scope. If multiple declarations are allowed, additional logic would be required to handle such scenarios appropriately.

## Interactions With Other Components

The `resolveLocal` function interacts primarily with the `CompilationState` class, which manages the current scope and all declared variables. By providing the index of a local variable, it enables other parts of the compiler to quickly access the variable's metadata or directly manipulate its value.

This function is crucial for the optimization phase of the compiler, where understanding the location of variables can help in generating more efficient machine code. Additionally, it supports error handling by allowing the compiler to report undefined variables when encountered during the compilation process.