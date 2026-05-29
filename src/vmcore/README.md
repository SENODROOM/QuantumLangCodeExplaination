# vmcore

The `vmcore` component is a central module in the Quantum Language compiler, responsible for executing bytecode and managing runtime operations efficiently. This directory includes several well-documented functions and files that together form the core of the virtual machine, ensuring its state remains consistent and enabling smooth execution.

## Overview

The `vmcore` component serves as the runtime environment where quantum programs are executed. It handles the interpretation of bytecode, manages memory allocation and deallocation, and processes various runtime operations such as function calls, error handling, and value manipulation.

### Key Components

- **run**: The main entry point for executing bytecode. It initializes the virtual machine and starts interpreting the bytecode instructions.
- **push**: Adds values to the stack, which is used for temporary storage during program execution.
- **pop**: Removes values from the stack, providing access to previously stored data.
- **runtimeError**: Handles runtime errors by printing error messages and potentially terminating the program.
- **toNumber**: Converts values to numbers, necessary for arithmetic operations.
- **valuesEqual**: Compares two values to determine if they are equal.
- **execBinary**: Executes binary operations like addition, subtraction, etc., on values.
- **execUnary**: Executes unary operations like negation or increment/decrement.
- **captureUpvalue**: Captures upvalues (local variables from enclosing scopes) in closures.
- **closeUpvalues**: Closes upvalues when a closure is no longer needed, freeing up memory.
- **callValue**: Calls a function or method using a value.
- **callClosure**: Calls a function using a closure.
- **callNativeFn**: Calls a native function implemented in C++.
- **callClass**: Instantiates a class and calls its methods.
- **callBuiltinMethod**: Calls built-in methods provided by the language runtime.

### File Structure

- **vmcore.h**: Header file containing declarations for all public functions and data structures in the `vmcore` component.
- **vmcore.cpp**: Source file implementing the functionality declared in `vmcore.h`.
- **stack.h**: Header file defining the stack data structure used by the virtual machine.
- **stack.cpp**: Source file implementing the stack operations.
- **memory.h**: Header file declaring memory management functions.
- **memory.cpp**: Source file implementing memory allocation and deallocation.
- **upvalues.h**: Header file defining upvalue-related data structures.
- **upvalues.cpp**: Source file implementing upvalue capture and closure management.
- **builtins.h**: Header file declaring built-in functions and methods.
- **builtins.cpp**: Source file implementing built-in functionalities.

## Flow Diagram

```plaintext
+-------------------+
|      run()        |
+---------+---------+
          |
          v
+---------+---------+
|     push()      |
+---------+---------+
          |
          v
+---------+---------+
|     pop()       |
+---------+---------+
          |
          v
+---------+---------+
|  runtimeError()   |
+---------+---------+
          |
          v
+---------+---------+
|    toNumber()     |
+---------+---------+
          |
          v
+---------+---------+
| valuesEqual()   |
+---------+---------+
          |
          v
+---------+---------+
| execBinary()    |
+---------+---------+
          |
          v
+---------+---------+
|  execUnary()    |
+---------+---------+
          |
          v
+---------+---------+
| captureUpvalue()|
+---------+---------+
          |
          v
+---------+---------+
| closeUpvalues() |
+---------+---------+
          |
          v
+---------+---------+
|   callValue()   |
+---------+---------+
          |
          v
+---------+---------+
|  callClosure()  |
+---------+---------+
          |
          v
+---------+---------+
| callNativeFn()  |
+---------+---------+
          |
          v
+---------+---------+
|   callClass()   |
+---------+---------+
          |
          v
+---------+---------+
| callBuiltinMethod()|
+-------------------+
```

## Usage

To use the `vmcore` component, include the `vmcore.h` header file in your project and link against the `vmcore.cpp` source file. Initialize the virtual machine by calling `run()` and start executing bytecode.

```cpp
#include "vmcore.h"

int main() {
    // Initialize the virtual machine
    run();

    // Execute bytecode
    // ...

    return 0;
}
```

For more detailed information on each function and its usage, refer to the individual documentation files within the `vmcore` directory.