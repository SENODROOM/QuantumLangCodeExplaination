# Quantum Language Compiler Core

## Overview

`src/compiler/CompilerCore.cpp` is a fundamental part of the Quantum Language compiler, tasked with transforming Abstract Syntax Trees (ASTs) into executable bytecode. This file encapsulates the primary logic for compiling various AST nodes and maintaining the state of the compilation process.

### Role in Compiler Pipeline

The `CompilerCore.cpp` serves as the heart of the compiler's compilation pipeline. It takes the root node of the AST and recursively compiles each child node until the entire tree is converted into bytecode. The resulting bytecode is stored in a `Chunk`, which is then executed by the virtual machine (VM).

### Key Design Decisions and Why

1. **Separation of Concerns**: By isolating the compilation logic into separate functions, such as `compileNode` and `compileBlock`, the codebase becomes more modular and easier to maintain. Each function handles a specific type of AST node or block, making it straightforward to understand and modify individual parts of the compiler without affecting others.

2. **Scope Management**: The `CompilerCore.cpp` implements scope management through the `beginScope` and `endScope` functions. These functions track the depth of nested scopes and manage the stack of local variables accordingly. This ensures that variables are correctly accessed and modified within their respective scopes, preventing issues like variable shadowing and out-of-scope errors.

3. **Symbol Resolution**: The compiler resolves symbols (variables and functions) using the `resolveLocal`, `resolveUpvalue`, and `addUpvalue` functions. These functions search for symbols in the current scope, then in enclosing scopes, and finally in the global scope. If a symbol is found, it emits the appropriate bytecode instruction to load or store the value. If not found, it adds the symbol to the global scope or creates an upvalue if necessary.

4. **Bytecode Emission**: The `emit` function is used to generate bytecode instructions. Depending on the operation required, it may emit instructions to load values, store values, perform arithmetic operations, call functions, and more. This abstraction allows the compiler to focus on the semantic meaning of the code rather than the low-level details of generating bytecode.

### Major Classes/Functions Overview

- **Compiler Class**:
  - **Constructor**: Initializes the compiler with a default state.
  - **compile Function**: Takes the root AST node and compiles it into bytecode, returning a shared pointer to the compiled chunk.
  - **beginScope Function**: Increases the scope depth, preparing the compiler for new local variables.
  - **endScope Function**: Decreases the scope depth and pops any unused local variables from the stack.
  - **declareLocal Function**: Declares a new local variable in the current scope.
  - **emitLoad Function**: Emits bytecode to load a value from a local or global variable.
  - **emitStore Function**: Emits bytecode to store a value into a local or global variable.

- **CompilerState Struct**:
  - Represents the state of the compiler during the compilation process.
  - Contains information about the current scope depth, local variables, and upvalues.

- **Op Enum**:
  - Defines the set of bytecode operations supported by the VM.
  - Operations include loading and storing values, performing arithmetic, calling functions, and more.

### Tradeoffs

- **Memory Usage**: Maintaining a stack of local variables and upvalues can lead to increased memory usage, especially in deeply nested scopes. However, this is necessary for correct symbol resolution and local variable access.
  
- **Complexity**: The separation of concerns and scope management introduce additional complexity to the compiler. While this makes the codebase more maintainable, it also requires careful handling to avoid bugs related to scope and symbol resolution.
  
- **Performance**: Emitting bytecode directly from the compiler can be slower compared to other approaches, such as intermediate representation (IR). However, this direct approach simplifies the execution phase and can result in faster overall performance.

In summary, `src/compiler/CompilerCore.cpp` plays a critical role in the Quantum Language compiler by providing the core logic for converting ASTs into bytecode. Its design decisions, including separation of concerns, scope management, and symbol resolution, ensure that the compiler is robust, maintainable, and performs well.