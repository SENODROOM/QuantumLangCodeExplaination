# vmcore

The `vmcore` component is a critical module within the Quantum Language compiler, designed to execute bytecode and manage runtime operations efficiently. This directory houses several well-documented functions and files that collaborate seamlessly to maintain the virtual machine's state and ensure smooth execution.

## Overview

The `vmcore` component is at the heart of the Quantum Language interpreter, handling the essential tasks of executing instructions, managing values on the stack, and resolving runtime errors. It consists of a series of functions that perform specific operations necessary for the virtual machine to function correctly.

### Key Functions

- **run**: Initiates the execution of the bytecode in the virtual machine.
- **push**: Adds a value to the top of the stack.
- **pop**: Removes the top value from the stack.
- **runtimeError**: Handles and reports runtime errors encountered during execution.
- **toNumber**: Converts a value to a number type.
- **valuesEqual**: Compares two values for equality.
- **execBinary**: Executes binary operations like addition or multiplication.
- **execUnary**: Executes unary operations like negation or increment.
- **captureUpvalue**: Captures upvalues (local variables from enclosing functions) into closures.
- **closeUpvalues**: Closes upvalues when a function exits.
- **callValue**: Calls a function using a value as its reference.
- **callClosure**: Calls a closure, which is a function bundled with an environment.
- **callNativeFn**: Invokes a native function implemented in C++.
- **callClass**: Instantiates and calls methods on a class.
- **callBuiltinMethod**: Calls built-in methods provided by the language.

### File Structure

The `vmcore` directory is organized as follows:

- **vmcore.h**: Header file containing declarations for all public functions and data structures used by the `vmcore` component.
- **vmcore.cpp**: Source file implementing the core functionality of the `vmcore` component.
- **stack.h** and **stack.cpp**: Files defining the stack data structure used by the virtual machine.
- **error.h** and **error.cpp**: Files handling error reporting and management.
- **types.h** and **types.cpp**: Files defining various data types and their conversions.
- **operations.h** and **operations.cpp**: Files implementing binary and unary operations.
- **closures.h** and **closures.cpp**: Files managing upvalues and closures.
- **calls.h** and **calls.cpp**: Files handling function calls, both native and user-defined.
- **classes.h** and **classes.cpp**: Files supporting object-oriented programming features like classes and method calls.

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
|  callValue()    |
+---------+---------+
          |
          v
+---------+---------+
| callClosure()   |
+---------+---------+
          |
          v
+---------+---------+
| callNativeFn()  |
+---------+---------+
          |
          v
+---------+---------+
|    callClass()    |
+---------+---------+
          |
          v
+---------+---------+
| callBuiltinMethod()|
+-------------------+
```

## Usage

To use the `vmcore` component, include the `vmcore.h` header file in your source code and link against the corresponding library. Below is a simple example demonstrating how to initialize and run the virtual machine:

```cpp
#include "vmcore.h"

int main() {
    VM* vm = createVM();
    loadBytecode(vm, "example.qb");
    run(vm);
    destroyVM(vm);
    return 0;
}
```

For more detailed information on each function and its usage, refer to the individual `.h` and `.cpp` files within the `vmcore` directory.

## Contributing

Contributions to the `vmcore` component are welcome! If you encounter any issues or have suggestions for improvements, please open an issue or submit a pull request on the [Quantum Language GitHub repository](https://github.com/quantum-lang/compiler).

---

This README provides a comprehensive overview of the `vmcore` component, detailing its purpose, key functions, and file structure. The flow diagram offers a visual representation of how these components interact, and the usage example demonstrates basic integration into a project.