# vmcore

The `vmcore` component is an essential module within the Quantum Language compiler, responsible for executing bytecode and managing runtime operations efficiently. This directory includes well-documented functions and files that form the core of the virtual machine, ensuring its robustness and enabling smooth bytecode execution.

## Overview

The `vmcore` component consists of several key components:

1. **Bytecode Execution**: Handles the interpretation and execution of bytecode instructions.
2. **Runtime Management**: Manages the runtime environment, including memory allocation, garbage collection, and error handling.
3. **Function Calls**: Implements mechanisms for calling user-defined and native functions.
4. **Upvalues**: Supports upvalues, which allow closures to capture variables from their enclosing scopes.

### File Structure

- **`vm.h`**: Header file containing declarations for all public functions in the `vmcore` component.
- **`vm.cpp`**: Source file implementing the functions declared in `vm.h`.
- **`bytecode.h`**: Header file defining the structure and constants related to bytecode instructions.
- **`bytecode.cpp`**: Source file providing implementations for parsing and executing bytecode.
- **`stack.h`**: Header file declaring stack-related functions and data structures.
- **`stack.cpp`**: Source file implementing stack operations such as push and pop.
- **`error.h`**: Header file defining error codes and messages used by the virtual machine.
- **`error.cpp`**: Source file handling runtime errors and exceptions.
- **`number.h`**: Header file providing utilities for converting values to numbers.
- **`number.cpp`**: Source file implementing number conversion functions.
- **`equality.h`**: Header file declaring functions for comparing values.
- **`equality.cpp`**: Source file implementing equality checks.
- **`binaryOps.h`**: Header file defining binary operation functions.
- **`binaryOps.cpp`**: Source file implementing binary arithmetic and logical operations.
- **`unaryOps.h`**: Header file declaring unary operation functions.
- **`unaryOps.cpp`**: Source file implementing unary arithmetic and logical operations.
- **`upvalues.h`**: Header file providing support for upvalues.
- **`upvalues.cpp`**: Source file implementing upvalue capture and closure management.
- **`garbageCollection.h`**: Header file defining garbage collection strategies.
- **`garbageCollection.cpp`**: Source file implementing memory management and garbage collection.
- **`functionCalls.h`**: Header file declaring function call handling functions.
- **`functionCalls.cpp`**: Source file implementing calls to both user-defined and native functions.
- **`classHandling.h`**: Header file providing support for class handling.
- **`classHandling.cpp`**: Source file implementing class creation, instantiation, and method invocation.

### Overall Flow

1. **Initialization**: The virtual machine initializes itself by setting up the necessary data structures and loading the bytecode.
2. **Bytecode Parsing**: The `bytecode.cpp` file parses the bytecode into executable instructions.
3. **Stack Operations**: The `stack.cpp` file manages the evaluation stack, pushing and popping values as needed during instruction execution.
4. **Instruction Execution**: The `vm.cpp` file executes bytecode instructions based on their opcodes. It handles binary and unary operations through `binaryOps.cpp` and `unaryOps.cpp`, respectively.
5. **Function Calls**: The `functionCalls.cpp` file processes function calls, whether they are user-defined or native, using mechanisms like `callValue`, `callClosure`, and `callNativeFn`.
6. **Upvalue Handling**: The `upvalues.cpp` file supports upvalues, allowing closures to maintain references to variables from their enclosing scopes.
7. **Garbage Collection**: The `garbageCollection.cpp` file manages memory allocation and garbage collection to prevent memory leaks and optimize performance.
8. **Error Handling**: The `error.cpp` file captures and handles runtime errors, ensuring the virtual machine can gracefully recover from unexpected situations.
9. **Finalization**: Upon completion of bytecode execution, the virtual machine cleans up resources and shuts down.

By following this structured approach, the `vmcore` component ensures efficient and reliable bytecode execution while maintaining a clean separation of concerns and modular design.