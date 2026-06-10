# QuantumLanguage Compiler - Vm.h

## Overview

The `include/Vm.h` header file is integral to the QuantumLanguage compiler, focusing on the definition of the Virtual Machine (VM). The VM executes compiled code efficiently, manages execution contexts, handles exceptions, and maintains the runtime environment, ensuring accurate and smooth program execution.

## Role in Compiler Pipeline

The VM operates at the execution phase of the compiler pipeline. After the compilation phase, which generates bytecode, the VM takes over to interpret and execute this bytecode. This separation allows for efficient execution and easier management of resources during runtime.

## Key Design Decisions and Why

1. **Separation of Concerns**: By isolating the execution logic into the VM, the compiler remains focused on generating optimal bytecode. This separation enhances modularity and maintainability.

2. **Efficient Memory Management**: Using smart pointers (`std::shared_ptr`) for managing heap-allocated objects like `Upvalue`, `Closure`, and `CallFrame` ensures automatic memory deallocation when these objects go out of scope, reducing the risk of memory leaks.

3. **Exception Handling**: The VM includes an exception handler mechanism, allowing it to manage errors gracefully. This feature is essential for building robust applications that can handle unexpected situations without crashing.

4. **Runtime Environment**: The VM maintains a runtime environment through its stack and frame structures, enabling dynamic function calls, local variable access, and method invocation.

## Major Classes/Functions Overview

### Upvalue

**Purpose**: Represents a captured variable from an enclosing function's scope. Used in closures to capture values that need to be accessible even after the enclosing function has finished execution.

**Key Functions**:
- `get()`: Retrieves the current value of the upvalue.
- `set(QuantumValue v)`: Updates the value of the upvalue.

### Closure

**Purpose**: Encapsulates a chunk of bytecode along with any upvalues it needs to access variables from its enclosing function's scope.

**Key Functions**:
- Constructor: Initializes the closure with a chunk and sets the closure's name based on the chunk's name.

### CallFrame

**Purpose**: Holds information about a function call, including the closure being called, the instruction pointer (IP), and the base index for local variables on the stack.

**Key Functions**:
- Constructor: Initializes the call frame with a closure and sets the initial IP and stack base index.

### ExceptionHandler

**Purpose**: Manages exception handling by storing the IP to jump to upon encountering an exception, the call-frame depth to unwind to, and the value stack depth to restore.

**Key Functions**:
- Constructor: Initializes the exception handler with the necessary details.

### VM Class

**Overview**: The core class of the VM, responsible for running bytecode, managing the stack, handling function calls, and implementing exception handling.

**Key Functions**:
- `run(std::shared_ptr<Chunk> chunk)`: Executes a top-level script represented by a chunk of bytecode.
- `registerNatives()`: Registers native functions that can be invoked from within the bytecode.
- `runFrame(size_t stopDepth = 0)`: Runs a single call frame until a specified stop depth is reached.
- `push(QuantumValue v)`: Pushes a value onto the stack.
- `pop()`: Pops a value from the stack.
- `peek(int offset = 0)`: Returns a reference to a value on the stack at a given offset.
- `callValue(QuantumValue callee, int argCount, int line)`: Handles calling a function or method using a value.
- `callClosure(std::shared_ptr<Closure> closure, int argCount, int line)`: Calls a closure with the specified arguments.
- `callNativeFn(std::shared_ptr<QuantumNative> fn, int argCount, int line)`: Invokes a native function with the provided arguments.
- `callClass(std::shared_ptr<QuantumClass> klass, int argCount, int line)`: Handles calling methods on a class instance.
- `callBuiltinMethod(QuantumValue receiver, std::string methodName, int argCount, int line)`: Calls a built-in method on a receiver object.

## Tradeoffs

1. **Complexity vs. Performance**: While the VM provides a high level of abstraction and flexibility, it also adds complexity to the compiler. Balancing these factors requires careful consideration of performance optimizations and maintainability.

2. **Memory Usage**: Smart pointers help manage memory more effectively, but they introduce additional overhead compared to raw pointers. Careful management of memory allocation and deallocation is essential to avoid excessive memory usage.

3. **Exception Handling Overhead**: Implementing comprehensive exception handling adds overhead to the execution process. However, it is crucial for building reliable applications, making the tradeoff worthwhile for many use cases.

4. **Flexibility vs. Simplicity**: Allowing for dynamic function calls and method invocations increases the flexibility of the VM but may complicate the implementation and debugging process. Finding a balance between flexibility and simplicity is important for practical use.

In conclusion, the `Vm.h` header file defines the Virtual Machine component of the QuantumLanguage compiler, providing essential functionality for executing compiled code efficiently and managing the runtime environment. Its design decisions balance complexity, performance, memory usage, and flexibility, making it a critical part of the compiler's architecture.