# Quantum Language Compiler - VmCore.cpp

## Overview

`VmCore.cpp` is a crucial component of the Quantum Language compiler's virtual machine (VM) core. It houses fundamental functions necessary for interpreting and executing compiled bytecode within the VM environment. This file plays a pivotal role in the compiler pipeline, especially during the execution phase where generated code is executed.

### Key Design Decisions and Why

1. **Iterator State Tagging**:
   - **Why**: To efficiently manage iterator states without additional data structures.
   - **Implementation**: Encodes iterators as `QuantumNative` objects with a function that never gets called. The VM recognizes these by their name prefix "__iter__" and stores an `IterState` keyed by the raw pointer.

2. **Stack Management**:
   - **Why**: To provide a flexible and efficient way to handle runtime values.
   - **Implementation**: Uses a dynamic array (`std::vector`) for the stack, with helper methods like `push`, `pop`, and `peek` to manage stack operations. The stack capacity is reserved up to 65536 elements to optimize performance.

3. **Runtime Error Handling**:
   - **Why**: To ensure robustness and provide meaningful error messages to users.
   - **Implementation**: Defines custom exceptions (`RuntimeError` and `TypeError`) to handle errors during execution. These exceptions include detailed error messages and line numbers for better debugging.

4. **Type Conversion**:
   - **Why**: To support type-safe operations between different value types.
   - **Implementation**: Provides a method `toNumber` to convert values to numbers, handling both numeric and string inputs. This ensures that arithmetic operations can be performed seamlessly across different types.

## Major Classes/Functions Overview

- **VM Class**:
  - **Constructor**: Initializes the global environment and registers native functions.
  - **Run Method**: Executes a given chunk of bytecode by setting up the initial stack frame and calling `runFrame`.

- **Stack Helpers**:
  - **Push**: Adds a value to the stack.
  - **Pop**: Removes and returns the top value from the stack.
  - **Peek**: Returns a reference to the value at a specified offset from the top of the stack.

- **Error Handling**:
  - **RuntimeError**: Custom exception class for runtime errors, including error message and line number.
  - **TypeError**: Custom exception class for type errors, providing context and line number information.

- **Type Conversion**:
  - **toNumber**: Converts a `QuantumValue` to a number, supporting both numeric and string inputs.

## Tradeoffs

- **Memory Usage vs. Performance**: Reserving stack capacity up to 65536 elements optimizes memory usage but may lead to higher peak memory consumption during certain executions.
  
- **Exception Overhead**: Using custom exceptions for error handling introduces overhead compared to standard exceptions. However, this approach provides more detailed error information, which aids in debugging.

- **Flexibility vs. Complexity**: Supporting multiple value types and complex operations increases flexibility but also adds complexity to the implementation.

Overall, `VmCore.cpp` is a well-designed module that balances functionality, efficiency, and maintainability, ensuring that the Quantum Language compiler can execute bytecode effectively and robustly.