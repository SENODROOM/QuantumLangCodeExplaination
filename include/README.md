# QuantumLanguage Compiler - Vm.h

## Overview

The `include/Vm.h` header file is a crucial component of the QuantumLanguage compiler, focusing on defining the virtual machine (VM) that executes compiled code. This VM plays a pivotal role in managing execution contexts, handling exceptions, and maintaining the runtime environment, thereby ensuring efficient and accurate execution of the program.

## Role in Compiler Pipeline

The VM acts as the heart of the QuantumLanguage interpreter, taking the output from the bytecode generation phase and executing it. It bridges the gap between the compiled code and the host environment, providing necessary functionalities such as memory management, function calling, and error handling.

### Key Design Decisions and Why

1. **Separation of Concerns**: By encapsulating the VM logic within its own class, the design ensures that the core components of the interpreter remain isolated from other parts of the compiler, enhancing maintainability and scalability.

2. **Runtime Environment Management**: The VM maintains a runtime environment including the value stack, call frames, and exception handlers. This design allows for dynamic changes during execution, accommodating features like recursion and exception handling.

3. **Efficient Memory Management**: Using smart pointers (`std::shared_ptr`, `std::unique_ptr`) for managing memory helps prevent memory leaks and dangling references, ensuring robustness and performance.

4. **Flexibility with Upvalues**: The VM supports upvalues, which are used to capture local variables from enclosing functions. This feature enables closures and provides flexibility in managing variable lifetimes.

## Major Classes/Functions Overview

### Upvalue
- **Purpose**: Represents a heap cell for captured variables, allowing them to be accessed even after they have left the stack scope.
- **Key Features**:
  - `cell`: A shared pointer to the live value.
  - `closed`: Storage for the value after it has been captured.

### Closure
- **Purpose**: Encapsulates a chunk of bytecode along with its upvalues and name.
- **Key Features**:
  - `chunk`: Shared pointer to the bytecode chunk.
  - `upvalues`: Vector of shared pointers to upvalues.
  - `name`: Name associated with the closure, typically the function name.

### CallFrame
- **Purpose**: Manages the execution context for each function call, storing information about the current function's closure, instruction pointer, and stack base.
- **Key Features**:
  - `closure`: Shared pointer to the current function's closure.
  - `ip`: Instruction pointer indicating the next bytecode instruction to execute.
  - `stackBase`: Index marking the beginning of local variables on the value stack.

### ExceptionHandler
- **Purpose**: Defines how the VM should handle exceptions, including the target IP to jump to and the depths of the call stack and value stack to unwind and restore.
- **Key Features**:
  - `catchIp`: Target IP for exception handling.
  - `frameDepth`: Depth of call frames to unwind upon exception.
  - `stackDepth`: Depth of the value stack to restore upon exception.

### VM Class
- **Overview**: The main class representing the virtual machine, responsible for running chunks of bytecode, managing the runtime environment, and handling exceptions.
- **Key Methods**:
  - `run(std::shared_ptr<Chunk> chunk)`: Executes a top-level script represented by a chunk of bytecode.
  - `registerNatives()`: Registers native functions that can be called from the bytecode.
  - `runFrame(size_t stopDepth = 0)`: Runs the current call frame until a specified stop depth or completion.
  - `push(QuantumValue v)`, `pop()`, `peek(int offset = 0)`: Manage the value stack, pushing values onto it, popping values off it, and peeking at values without removing them.
  - `callValue(QuantumValue callee, int argCount, int line)`, `callClosure(std::shared_ptr<Closure> closure, int argCount, int line)`, `callNativeFn(std::shared_ptr<QuantumNative> fn, int argCount, int line)`, `callClass(std::shared_ptr<QuantumClass> klass, int argCount, int line)`, `callBuiltinMethod(...)`: Handle different types of function calls, including native functions, closures, and built-in methods.

## Tradeoffs

1. **Memory Overhead**: Using smart pointers introduces some overhead compared to raw pointers, but enhances safety and reduces manual memory management errors.

2. **Complexity**: The separation of concerns into multiple classes increases complexity, making the codebase harder to understand and maintain. However, it also improves modularity and reusability.

3. **Performance**: While the use of smart pointers adds some overhead, the overall design aims to provide a flexible and safe runtime environment, which can lead to better performance through improved error handling and easier debugging.

4. **Resource Usage**: Managing resources explicitly via call frames and upvalues requires careful handling to avoid resource leaks or premature deallocation, impacting both memory usage and performance.

By understanding these aspects, developers can effectively utilize the VM class and related structures to build powerful and reliable interpreters for QuantumLanguage.