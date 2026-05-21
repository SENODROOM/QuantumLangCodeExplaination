# vmcore

The `vmcore` component is a vital module in the Quantum Language compiler responsible for executing bytecode and managing runtime operations efficiently. This directory includes several well-documented functions and files that work together to maintain the virtual machine's state and ensure smooth execution.

## Overview

The `vmcore` component serves as the core engine of the Quantum Language interpreter. It handles the execution of bytecode instructions, manages memory allocation and deallocation, and provides mechanisms for calling native functions and methods. The component consists of several key files:

- **bytecode.h**: Contains definitions for bytecode instructions and their corresponding opcodes.
- **stack.h**: Manages the stack used for function calls and local variables.
- **memory.h**: Handles dynamic memory allocation and garbage collection.
- **closure.h**: Defines structures and functions related to closures, which are first-class functions with access to their lexical environment.
- **class.h**: Manages class definitions and instances, providing support for object-oriented programming features.
- **builtin.h**: Includes declarations for built-in functions and methods that can be called directly from the language.

### Key Functions

- **run()**: Initializes the virtual machine and starts the execution of the provided bytecode.
- **push()** and **pop()**: Manage the stack, allowing values to be pushed onto and popped from it during function calls.
- **runtimeError()**: Handles runtime errors, such as division by zero or undefined variables, by printing an error message and exiting the program.
- **closeUpvalues()**: Closes upvalues when a function returns, ensuring that any references to local variables are properly cleaned up.
- **callValue()**, **callClosure()**, and **callNativeFn()**: Implement different ways to call functions, including regular function calls, closure calls, and calls to native functions written in C++.
- **callClass()** and **callBuiltinMethod()**: Handle method calls on objects and built-in methods respectively, providing the necessary functionality for object-oriented programming.

## File Structure

```
vmcore/
├── bytecode.h
├── stack.h
├── memory.h
├── closure.h
├── class.h
├── builtin.h
└── vm.cpp
```

### bytecode.h

Contains definitions for bytecode instructions and their corresponding opcodes. Each instruction is represented by a unique opcode, making it easy to decode and execute them.

### stack.h

Manages the stack used for function calls and local variables. The stack supports push and pop operations, ensuring that values are correctly managed during function execution.

### memory.h

Handles dynamic memory allocation and garbage collection. The component uses reference counting to manage memory, automatically deallocating memory when it is no longer in use.

### closure.h

Defines structures and functions related to closures, which are first-class functions with access to their lexical environment. Closures allow functions to capture and use variables from their surrounding scope.

### class.h

Manages class definitions and instances, providing support for object-oriented programming features. Classes can have properties, methods, and constructors, allowing for the creation of complex data structures and behaviors.

### builtin.h

Includes declarations for built-in functions and methods that can be called directly from the language. These functions provide essential functionality for tasks like input/output operations, arithmetic calculations, and more.

### vm.cpp

Implements the main logic of the virtual machine, coordinating the execution of bytecode and the management of runtime resources. This file includes the implementation of the `run()` function, which initializes the virtual machine and starts the execution process.

## Usage

To use the `vmcore` component, include the necessary headers in your project and link against the appropriate libraries. Here is a basic example of how to run bytecode using the `vmcore` component:

```cpp
#include "vmcore/bytecode.h"
#include "vmcore/vm.h"

int main() {
    // Load bytecode into the virtual machine
    loadBytecode("example.qb");

    // Run the bytecode
    run();

    return 0;
}
```

In this example, the `loadBytecode()` function loads the bytecode into the virtual machine, and the `run()` function executes it.

## Contributing

Contributions to the `vmcore` component are welcome! If you find a bug or have an idea for a new feature, please open an issue or submit a pull request. Make sure to follow the existing coding style and guidelines to ensure seamless integration with the rest of the Quantum Language compiler.

---

This README provides a comprehensive overview of the `vmcore` component, highlighting its role in the Quantum Language compiler and outlining the structure and usage of its key files and functions.