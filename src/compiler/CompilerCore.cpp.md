# Quantum Language Compiler Core

## Overview

`src/compiler/CompilerCore.cpp` is the central component of the Quantum Language compiler responsible for converting Abstract Syntax Trees (ASTs) into executable bytecode. It handles the core logic of compiling different AST nodes and manages the state of the compilation process.

### Role in Compiler Pipeline

In the Quantum Language compiler pipeline, `CompilerCore.cpp` acts as the intermediary between the parser and the virtual machine (VM). The parser generates the AST, which is then passed to `CompilerCore.cpp` for compilation. After the entire program or block has been compiled, the resulting bytecode is ready to be executed by the VM.

## Key Design Decisions and Why

1. **Separation of Concerns**: By isolating the compilation logic within `CompilerCore.cpp`, the codebase remains organized and easier to maintain. Each function focuses on a specific aspect of compilation, such as handling variable declarations or emitting bytecode instructions.

2. **Scoping Mechanism**: The compiler maintains a stack-based scoping mechanism using `CompilerState`. This allows it to keep track of local variables and their depths, ensuring that correct scope resolution is maintained during compilation.

3. **Upvalue Handling**: To support closures and nested functions, `CompilerCore.cpp` implements an upvalue system. Upvalues are used to capture variables from outer scopes, allowing them to be accessed within inner scopes.

4. **Bytecode Emission**: The compiler emits bytecode instructions directly into the current chunk being processed. This approach ensures efficient memory management and fast compilation times.

## Major Classes/Functions Overview

### Compiler Class

- **Constructor**: Initializes the compiler with a default state.
- **compile(ASTNode &root)**: Compiles the root node of the AST and returns the resulting chunk.
- **beginScope()**: Marks the beginning of a new scope.
- **endScope(int line)**: Ends the current scope and cleans up locals and upvalues.
- **resolveLocal(CompilerState *state, const std::string &name)**: Resolves a local variable by its name and depth.
- **addUpvalue(CompilerState *state, int index, bool isLocal)**: Adds an upvalue to the current chunk.
- **resolveUpvalue(CompilerState *state, const std::string &name)**: Resolves an upvalue by its name.
- **declareLocal(const std::string &name, int)**: Declares a local variable within the current scope.
- **emitLoad(const std::string &name, int line)**: Emits a load instruction for a variable.
- **emitStore(const std::string &name, int line)**: Emits a store instruction for a variable.

### CompilerState Struct

- **scopeDepth**: Tracks the current depth of the scope.
- **locals**: A list of local variables currently in scope.
- **upvalues**: A list of upvalues captured by the current function.
- **chunk**: The bytecode chunk being compiled.

## Tradeoffs

1. **Memory Management**: While the direct emission of bytecode into chunks is efficient, it requires careful management of memory to ensure that the chunk does not grow uncontrollably. This can lead to additional complexity in the compiler's implementation.

2. **Scope Resolution**: The stack-based scoping mechanism simplifies the resolution of local variables but may introduce overhead when dealing with deeply nested scopes. Additionally, resolving upvalues adds another layer of complexity.

3. **Performance vs. Simplicity**: The separation of concerns and clear structure of `CompilerCore.cpp` make the code more readable and maintainable, but at the cost of potentially increased performance overhead due to additional function calls and checks.

Overall, `src/compiler/CompilerCore.cpp` provides a robust foundation for the Quantum Language compiler, balancing functionality, simplicity, and performance considerations.