# CompilerCore

The `CompilerCore` component is a crucial part of the Quantum Language compiler, designed to handle essential tasks throughout the compilation process. It manages scopes, resolves variable references, and generates bytecode suitable for execution in the quantum runtime environment. This module acts as the central nervous system for processing quantum nodes and expressions, ensuring accurate and efficient compilation.

## Overview

- **Scopes**: The `CompilerCore` maintains a stack of scopes to manage variable bindings and lifetimes. Scopes are nested, allowing for block-level scoping.
- **Variable Resolution**: It resolves local and upvalue variables, ensuring that references are correctly mapped to their definitions.
- **Bytecode Emission**: The core emits bytecode instructions based on the parsed quantum language constructs. These instructions are then executable by the quantum runtime.

## Files and Functions

### compile.cpp
- **Functionality**: Handles the main compilation loop, invoking other functions to process individual nodes.
- **Functions**:
  - `compile()`: Main entry point for compiling the quantum program.
  - `compileNode(Node* node)`: Compiles a single quantum node.
  - `visit(Node* node)`: Visits a node and processes it according to its type.

### scope.cpp
- **Functionality**: Manages the creation, destruction, and resolution of scopes.
- **Functions**:
  - `beginScope()`: Begins a new scope.
  - `endScope()`: Ends the current scope.
  - `declareLocal(const std::string& name)`: Declares a local variable within the current scope.
  - `resolveLocal(const std::string& name)`: Resolves a local variable reference.

### upvalue.cpp
- **Functionality**: Manages upvalues, which are variables from outer scopes that need to be captured by closures.
- **Functions**:
  - `addUpvalue(const std::string& name)`: Adds an upvalue to the current function's list.
  - `resolveUpvalue(const std::string& name)`: Resolves an upvalue reference.

### emit.cpp
- **Functionality**: Emits bytecode instructions into the output buffer.
- **Functions**:
  - `emitLoad(const std::string& name)`: Emits an instruction to load a variable onto the stack.
  - `emitStore(const std::string& name)`: Emits an instruction to store a value from the stack into a variable.

### loop.cpp
- **Functionality**: Manages loop structures, including begin and end loop logic.
- **Functions**:
  - `beginLoop()`: Marks the beginning of a loop.
  - `emitBreak()`: Emits an instruction to break out of a loop.
  - `emitContinue()`: Emits an instruction to continue to the next iteration of a loop.
  - `endLoop()`: Marks the end of a loop.

### block.cpp
- **Functionality**: Processes quantum blocks, which are groups of statements.
- **Functions**:
  - `compileBlock(Block* block)`: Compiles a quantum block.

### expr.cpp
- **Functionality**: Processes quantum expressions.
- **Functions**:
  - `compileExpr(Expr* expr)`: Compiles a quantum expression.

## Overall Flow

1. **Compilation Entry Point**: The `compile()` function starts the compilation process by iterating over the top-level quantum nodes.
2. **Scope Management**: As nodes are processed, `beginScope()` and `endScope()` manage the nesting of scopes.
3. **Variable Declaration and Resolution**: Variables are declared using `declareLocal()` and resolved using `resolveLocal()` or `resolveUpvalue()`.
4. **Bytecode Generation**: Instructions are emitted using `emitLoad()`, `emitStore()`, etc., based on the nodes being processed.
5. **Loop Handling**: Loop structures are managed using `beginLoop()`, `emitBreak()`, `emitContinue()`, and `endLoop()`.
6. **Block Processing**: Blocks of statements are compiled using `compileBlock()`.
7. **Expression Compilation**: Expressions are processed using `compileExpr()`.

By following this structured approach, the `CompilerCore` ensures that the quantum program is compiled accurately and efficiently, preparing it for execution in the quantum runtime environment.