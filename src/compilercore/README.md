# CompilerCore

The `CompilerCore` component is an integral part of the Quantum Language compiler, responsible for handling fundamental tasks during the compilation process. It manages scopes, resolves variable references, and emits bytecode that can be executed by the quantum runtime environment. This module serves as the central nervous system for processing quantum nodes and expressions, ensuring their accurate parsing and transformation into executable code.

## Overview

- **Scopes Management**: The `CompilerCore` maintains a stack of scopes to keep track of variable declarations and their visibility.
- **Variable Resolution**: Functions like `resolveLocal`, `addUpvalue`, and `resolveUpvalue` help in identifying where a variable is declared and whether it needs to be accessed through upvalues.
- **Bytecode Emission**: Methods such as `emitLoad` and `emitStore` generate bytecode instructions for loading and storing values.
- **Control Flow**: Functions like `beginLoop`, `emitBreak`, `emitContinue`, and `endLoop` manage control structures within the quantum program.

## Files and Their Roles

### compile.cpp
- **Functionality**: Contains the main entry point for compiling quantum programs.
- **Relationships**: Interacts with all other core components to orchestrate the compilation process.

### scope.cpp
- **Functionality**: Manages the creation, destruction, and resolution of variable scopes.
- **Relationships**: Used by `compile.cpp` and other components to maintain scope information.

### resolve.cpp
- **Functionality**: Handles the resolution of local and upvalue variables.
- **Relationships**: Utilized by various components to ensure correct variable access.

### emit.cpp
- **Functionality**: Generates bytecode instructions for operations like load and store.
- **Relationships**: Called by expression and statement compilers to produce executable code.

### loop.cpp
- **Functionality**: Manages the generation of bytecode for loop constructs.
- **Relationships**: Invoked by the block and expression compilers to handle loops.

### node.cpp
- **Functionality**: Defines and processes quantum nodes and expressions.
- **Relationships**: Central to the compilation process, interacting with all other components.

## Overall Flow

1. **Initialization**: The compiler initializes the `CompilerCore` and sets up the necessary data structures.
2. **Scope Management**: As the compiler encounters variable declarations, it uses `beginScope` and `declareLocal` to manage scopes.
3. **Variable Resolution**: During the compilation of expressions, the `CompilerCore` resolves variable references using `resolveLocal` and `resolveUpvalue`.
4. **Bytecode Generation**: Expressions and statements are compiled, generating bytecode via `emitLoad`, `emitStore`, etc.
5. **Control Flow**: Loops and conditionals are processed, with bytecode generated for break, continue, and end conditions using `beginLoop`, `emitBreak`, `emitContinue`, and `endLoop`.
6. **Finalization**: After all nodes have been processed, the `CompilerCore` finalizes the compilation, emitting any remaining bytecode and cleaning up resources.

By following this structured approach, the `CompilerCore` ensures that quantum programs are compiled efficiently and correctly, providing a solid foundation for further optimization and execution.