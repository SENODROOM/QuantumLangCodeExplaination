# Quantum Language Compiler Core

## Overview

`src/compiler/CompilerCore.cpp` is a crucial component of the Quantum Language compiler responsible for converting Abstract Syntax Trees (ASTs) into executable bytecode. This file centralizes the core logic required to compile different AST nodes and manage the state of the compilation process.

### Role in Compiler Pipeline

In the Quantum Language compiler pipeline, `CompilerCore.cpp` acts as the heart of the compilation process. It starts by initializing a new `CompilerState` object representing the script context. The compiler then processes the root AST node, which could be either a block statement or another type of node. If it's a block statement, the compiler calls `compileBlock`, otherwise, it directly compiles the node using `compileNode`. After processing the root node, the compiler emits a `RETURN_NIL` instruction to indicate the end of execution. Finally, it returns the compiled chunk.

### Key Design Decisions and Why

#### Scope Management
- **Design Decision**: The compiler maintains a stack-based scope system where each scope has its own set of local variables.
- **Why**: This approach allows for dynamic scoping and nested scopes, essential features in many programming languages.

#### Upvalues
- **Design Decision**: To support closures, the compiler uses upvalues—references to variables in outer scopes.
- **Why**: Upvalues enable functions to capture and access variables from their enclosing scopes, providing flexibility and power in functional programming.

#### Error Handling
- **Design Decision**: The compiler throws exceptions when errors occur during the compilation process.
- **Why**: Exceptions provide a robust way to handle errors gracefully, ensuring that the compiler can stop at any point and report issues clearly.

### Major Classes/Functions Overview

#### Compiler Class
- **Overview**: Manages the overall compilation process, including error handling and state management.
- **Key Functions**:
  - `compile(ASTNode &root)`: Compiles the entire AST starting from the root node.
  - `emit(Op op, int arg1, int arg2)`: Emits bytecode instructions to the current chunk.
  - `emitLoad(const std::string &name, int line)`: Emits instructions to load a variable from locals or globals.
  - `emitStore(const std::string &name, int line)`: Emits instructions to store a value into a variable.

#### CompilerState Struct
- **Overview**: Represents the state of the compiler at a given point in time, including the current chunk, scope depth, and local variables.
- **Key Members**:
  - `chunk`: The current chunk being compiled.
  - `scopeDepth`: The depth of the current scope.
  - `locals`: A list of local variables within the current scope.

#### Local Variable Struct
- **Overview**: Describes a local variable, including its name, depth, and whether it's captured by a closure.
- **Key Members**:
  - `name`: The name of the local variable.
  - `depth`: The depth of the scope in which the variable is declared.
  - `isCaptured`: Indicates whether the variable is captured by a closure.

#### Upvalue Struct
- **Overview**: Represents an upvalue used in closures, indicating whether it refers to a local or global variable.
- **Key Members**:
  - `isLocal`: Whether the upvalue refers to a local variable.
  - `index`: The index of the variable in the enclosing scope or chunk.

### Tradeoffs

#### Memory Usage vs. Performance
- **Tradeoff**: The use of upvalues adds overhead to memory usage due to additional data structures but improves performance by allowing efficient access to variables in outer scopes.

#### Flexibility vs. Complexity
- **Tradeoff**: Supporting dynamic scoping increases the flexibility of the language but also complicates the compiler implementation, requiring careful handling of scope depths and upvalues.

#### Error Handling vs. Robustness
- **Tradeoff**: Throwing exceptions simplifies error reporting but may lead to higher runtime costs compared to other error handling mechanisms like returning error codes.

Overall, `CompilerCore.cpp` plays a pivotal role in transforming source code into executable bytecode, managing scope and upvalues, and handling errors effectively. Its design decisions balance flexibility, performance, and robustness, making it a vital part of the Quantum Language compiler.