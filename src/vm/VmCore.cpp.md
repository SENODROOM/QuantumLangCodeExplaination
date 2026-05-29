# Quantum Language Compiler - VmCore.cpp

## Overview

`VmCore.cpp` is a central component of the Quantum Language compiler, focusing on the virtual machine (VM) core responsible for interpreting and executing compiled bytecode. This file plays a pivotal role in the execution phase of the compiler pipeline, ensuring efficient and accurate code execution.

### Key Design Decisions and Why

1. **Iterator State Tagging**:
   - **Why**: To manage iterator states effectively, we use a unique encoding strategy where iterators are represented as `QuantumNative` objects with a specific function behavior (`fn()` never gets called).
   - **Implementation**: The VM identifies these iterators through a naming convention (`__iter__`) and stores their state using a `std::unordered_map`.

2. **Stack Management**:
   - **Why**: Efficient stack management is crucial for performance and correctness. By pre-reserving a large capacity for the stack, we reduce reallocations and improve overall execution speed.
   - **Implementation**: The stack is dynamically resized based on the program's needs, but a minimum capacity of 65536 elements is always reserved.

3. **Error Handling**:
   - **Why**: Robust error handling ensures that the VM can gracefully handle exceptions and provide meaningful feedback to the user.
   - **Implementation**: Custom exception types (`RuntimeError`, `TypeError`) are defined to encapsulate different error scenarios, allowing for clear and consistent error messages.

### Major Classes/Functions Overview

- **VM Class**:
  - **Role**: Manages the entire execution process, including running chunks, managing the call stack, and handling errors.
  - **Key Functions**:
    - `VM()`: Initializes the VM with default settings.
    - `run(std::shared_ptr<Chunk> chunk)`: Starts the execution of a given chunk of bytecode.
    - `push(QuantumValue v)`, `pop()`, `peek(int offset)`: Manage the VM's operand stack.
    - `runtimeError(const std::string &msg, int line)`: Throws runtime errors with context information.
    - `toNumber(const QuantumValue &v, const std::string &ctx, int line)`: Converts values to numbers, handling type mismatches gracefully.

- **QuantumValue Class**:
  - **Role**: Represents any value that can be manipulated within the VM, supporting various data types like numbers, strings, booleans, and nil.
  - **Key Functions**:
    - `isNumber()`, `isString()`, `isBool()`, `isNil()`: Check the type of the value.
    - `asNumber()`, `asString()`, `asBool()`: Convert the value to its respective type.

- **Closure Class**:
  - **Role**: Encapsulates a chunk of bytecode along with its enclosing environment, forming the basis of function closures.
  - **Key Functions**:
    - `Closure(std::shared_ptr<Chunk> chunk)`: Initializes a new closure with the specified chunk.

### Tradeoffs

- **Memory Usage vs Performance**:
  - **Pre-reservation of Stack Capacity**: While it improves performance by reducing reallocations, it also increases memory usage, especially for programs with small stack requirements.

- **Type Safety vs Convenience**:
  - **Dynamic Type Conversion**: Allowing dynamic type conversion between numbers and strings provides convenience but sacrifices some level of type safety, which must be handled carefully to avoid runtime errors.

- **Debugging Complexity vs Execution Speed**:
  - **Conditional Compilation for Debugging**: Enabling debug tracing (`DEBUG_TRACE_EXECUTION`) adds complexity to the codebase but allows for easier debugging and verification of the VM's behavior.

Overall, `VmCore.cpp` is designed to balance performance, robustness, and ease of development while providing a solid foundation for executing quantum language programs efficiently.