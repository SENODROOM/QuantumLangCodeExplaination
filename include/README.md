# QuantumLanguage Compiler - Vm.h

## Overview

The `include/Vm.h` header file is an essential part of the QuantumLanguage compiler, dedicated to defining the virtual machine (VM) responsible for executing compiled code. The VM manages execution contexts, handles exceptions, and maintains the runtime environment, ensuring efficient and accurate execution of the program.

## Role in Compiler Pipeline

The VM acts as the final stage in the compiler pipeline, taking the output from the bytecode generation phase and running it. It interprets the bytecode instructions, interacts with the heap and stack, and manages function calls, local variables, and upvalues.

## Key Design Decisions and Why

1. **Stack-Based Architecture**: The VM uses a stack-based architecture for managing local variables and function arguments. This decision simplifies memory management and supports dynamic scoping.

2. **Heap-Allocated Values**: Variables that need to be captured or passed between functions are stored on the heap using shared pointers. This allows for garbage collection and ensures that values remain valid even when they leave the stack scope.

3. **Exception Handling**: The VM includes a mechanism for handling exceptions through structured exception handlers. Each handler specifies the IP to jump to upon exception, the call-frame depth to unwind, and the value stack depth to restore, providing robust error management capabilities.

4. **Open Upvalues List**: To support closures and upvalues, the VM maintains an open upvalues list. This list helps manage the lifecycle of upvalues, ensuring they are properly closed when their enclosing function exits.

## Major Classes/Functions Overview

### Upvalue
- **Role**: Represents a captured variable that needs to be accessed from a nested function.
- **Structure**:
  ```cpp
  struct Upvalue {
      std::shared_ptr<QuantumValue> cell; // Points to the live value
      QuantumValue closed;                // Storage after variable leaves stack

      explicit Upvalue(std::shared_ptr<QuantumValue> c) : cell(c) {}

      QuantumValue get() const { return *cell; }
      void set(QuantumValue v) { *cell = std::move(v); }
  };
  ```

### Closure
- **Role**: Encapsulates a chunk of bytecode along with its upvalues.
- **Structure**:
  ```cpp
  struct Closure {
      std::shared_ptr<Chunk> chunk;
      std::vector<std::shared_ptr<Upvalue>> upvalues;
      std::string name;

      explicit Closure(std::shared_ptr<Chunk> c)
          : chunk(std::move(c)), name(chunk->name) {}
  };
  ```

### CallFrame
- **Role**: Manages the context of a function call, including the closure, instruction pointer, and stack base.
- **Structure**:
  ```cpp
  struct CallFrame {
      std::shared_ptr<Closure> closure;
      size_t ip;        // Instruction pointer
      size_t stackBase; // Where locals start on the value stack
  };
  ```

### ExceptionHandler
- **Role**: Defines how exceptions should be handled, including the target IP and stack depths to restore.
- **Structure**:
  ```cpp
  struct ExceptionHandler {
      int32_t catchIp;   // IP to jump to on exception
      size_t frameDepth; // Call-frame depth to unwind to
      size_t stackDepth; // Value stack depth to restore
  };
  ```

### VM Class
- **Overview**: The main class representing the Virtual Machine.
- **Public Methods**:
  - `run(std::shared_ptr<Chunk> chunk)`: Executes a compiled chunk (top-level script).
  - `registerNatives()`: Registers native functions with the VM.
  
- **Private Members**:
  - `stack_`: The value stack used for storing local variables and function arguments.
  - `frames_`: A vector of call frames, each representing a function call.
  - `handlers_`: A vector of exception handlers, managing exception propagation.
  - `openUpvalues_`: A list of open upvalues for managing closure lifetimes.
  - `stepCount_`: Counts the number of executed steps, limiting the runtime to prevent infinite loops.
  - `pendingInstances_`: Stores instances that need to be instantiated during execution.

## Tradeoffs

1. **Memory Management**: Using shared pointers for heap-allocated values provides automatic garbage collection but may introduce overhead compared to manual memory management.

2. **Exception Handling**: Structured exception handling adds complexity but ensures robust error management and prevents crashes due to unhandled exceptions.

3. **Performance**: The stack-based architecture is generally fast but can lead to increased memory usage for deep recursion or large data structures.

By understanding these components and their roles within the QuantumLanguage compiler, developers can better appreciate the design choices made and how they contribute to the overall efficiency and reliability of the system.