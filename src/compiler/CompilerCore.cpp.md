# Quantum Language Compiler Core

## Overview

`src/compiler/CompilerCore.cpp` is the central component of the Quantum Language compiler, responsible for converting Abstract Syntax Trees (ASTs) into executable bytecode. This file encapsulates the core logic necessary to handle various AST nodes and manage the state of the compilation process efficiently.

### Role in Compiler Pipeline

The `CompilerCore.cpp` plays a pivotal role in the Quantum Language compiler's pipeline:

1. **Parsing**: The AST is generated from the source code using a parser.
2. **Compilation**: `CompilerCore.cpp` processes the AST and converts it into bytecode.
3. **Bytecode Execution**: The resulting bytecode is executed by the Virtual Machine (VM).

This core component ensures that the entire compilation process is seamless and efficient, allowing the VM to execute quantum programs effectively.

### Key Design Decisions and Why

#### Scope Management

- **Scopes**: Each function or block has its own scope, managed through the `CompilerState`.
- **Resolution**: Variables are resolved within their respective scopes, falling back to enclosing scopes if not found locally.
- **Trade-offs**: While this approach provides flexibility, it can lead to increased complexity in managing variable lifetimes and captures.

#### Bytecode Emission

- **Op Codes**: A set of op codes (`Op`) defines the operations that can be performed on the stack during execution.
- **Dynamic Memory Allocation**: Bytecode is dynamically allocated as the compiler progresses through the AST.
- **Performance**: Dynamic allocation allows for efficient memory usage but requires careful management to avoid leaks.

#### Error Handling

- **Exceptions**: Exceptions are used to propagate errors throughout the compilation process.
- **Detailed Messages**: Errors include detailed messages indicating where and why they occurred.
- **Trade-offs**: Exception handling simplifies error propagation but can impact performance compared to more explicit control flow mechanisms.

### Major Classes/Functions Overview

#### Compiler Class

- **Constructor**: Initializes the compiler with a default state.
- **compile**: Main entry point for compiling an AST. Handles the root node and emits the final `RETURN_NIL` instruction.

#### CompilerState Struct

- **Scope Depth**: Tracks the depth of the current scope.
- **Locals Vector**: Stores information about local variables within the current scope.
- **Upvalues Vector**: Manages upvalues for capturing variables from outer scopes.

#### LocalInfo Struct

- **Name**: Variable name.
- **Depth**: Scope depth at which the variable is declared.
- **Is Captured**: Indicates whether the variable is captured in an upvalue.

#### UpvalueInfo Struct

- **Index**: Index of the variable in the enclosing chunk.
- **Is Local**: Indicates whether the variable is local to the enclosing scope.

#### Helper Functions

- **beginScope**: Increments the scope depth when entering a new scope.
- **endScope**: Decrements the scope depth and cleans up locals and upvalues.
- **resolveLocal**: Searches for a local variable by name.
- **addUpvalue**: Adds an upvalue to the current chunk.
- **resolveUpvalue**: Searches for an upvalue by name.
- **declareLocal**: Declares a local variable within the current scope.
- **emitLoad**: Emits instructions to load a variable onto the stack.
- **emitStore**: Emits instructions to store a value into a variable.

### Tradeoffs

- **Flexibility vs. Complexity**: Managing multiple nested scopes adds complexity to the compiler but enhances its ability to handle complex programs.
- **Memory Usage**: Dynamic memory allocation for bytecode can lead to higher memory usage but is generally more efficient than preallocating fixed-size buffers.
- **Error Handling**: Using exceptions simplifies error propagation but may have performance implications compared to other error handling strategies.

Overall, `src/compiler/CompilerCore.cpp` is a critical part of the Quantum Language compiler, balancing functionality, efficiency, and simplicity to ensure robust and performant bytecode generation.