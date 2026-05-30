# Quantum Language Compiler: Statements Compilation

## Overview

The `src/compiler/CompilerStatements.cpp` file plays a crucial role in the Quantum Language compiler's pipeline, focusing on the compilation of statements like variable declarations, function declarations, and class declarations. It ensures that high-level quantum language code is efficiently transformed into bytecode that can be executed by the virtual machine.

### Key Design Decisions and Why

1. **Scope Management**: The compiler maintains separate scopes for global and local variables. This decision allows for clear separation between different parts of the program, preventing name clashes and enabling proper scoping rules.
   
2. **Bytecode Emission**: Instead of generating intermediate representations directly, the compiler emits bytecode operations (`Op`). This approach simplifies the compilation process and makes it easier to optimize and execute the generated code.

3. **Upvalues Handling**: For closures, the compiler handles upvalues separately. Upvalues refer to variables from enclosing functions that are captured by nested functions. This decision ensures that closures are correctly implemented and that memory management is efficient.

4. **Inheritance Support**: Classes can inherit from other classes. The compiler supports inheritance by emitting specific bytecode operations (`Op::MAKE_CLASS`, `Op::INHERIT`) that handle the creation and setup of class instances.

## Major Classes/Functions Overview

### Compiler Class

- **Role**: Manages the overall compilation process, including parsing, semantic analysis, and bytecode generation.
- **Key Functions**:
  - `compileVarDecl`: Compiles variable declarations, handling both initialized and uninitialized cases.
  - `compileFunctionDecl`: Compiles function declarations, creating closures when necessary.
  - `compileClassDecl`: Compiles class declarations, supporting inheritance and method binding.

### Error Class

- **Role**: Handles errors during the compilation process, providing informative error messages and locations.
- **Key Functions**: Various methods to report errors at different stages of compilation.

### Vm Class

- **Role**: Represents the virtual machine where the compiled bytecode will run. Provides functionality for executing bytecode and managing runtime state.
- **Key Methods**: `execute`, `loadConst`, `defineGlobal`, etc., which correspond to bytecode operations.

### Helper Functions

- **`emit`**: Emits a single bytecode operation.
- **`emitLoad`**: Loads a value from a variable or expression.
- **`declareLocal`**: Declares a new local variable.
- **`resolveLocal`**: Resolves the slot index of a local variable within the current scope.

## Tradeoffs

1. **Complexity vs. Performance**: By using bytecode emission, the compiler achieves simplicity in the compilation process but may sacrifice some performance compared to direct IR generation.

2. **Memory Management**: Handling upvalues requires additional complexity in the compiler to manage memory allocation and deallocation for closures.

3. **Scalability**: The separation of scopes and bytecode emission provides scalability, allowing the compiler to handle larger programs with multiple scopes and complex control structures.

Overall, the `src/compiler/CompilerStatements.cpp` file is essential for transforming high-level quantum language code into executable bytecode, ensuring proper scoping, function handling, and class inheritance support.