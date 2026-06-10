# QuantumLanguage Compiler - Compiler.h

## Overview

The `include/Compiler.h` header file is integral to the QuantumLanguage compiler, focusing on the core functionality of compiling source code into executable chunks. This file defines essential components such as the Virtual Machine (VM), scope management, and various helper functions necessary for the compilation process.

## Role in Compilation Pipeline

The `Compiler.h` file serves as a central component within the QuantumLanguage compiler's pipeline. It handles the translation of abstract syntax trees (ASTs) into intermediate representation (IR) chunks that can be executed by the VM. This includes parsing, semantic analysis, and code generation phases.

### Key Design Decisions and Why

1. **Virtual Machine (VM)**: The VM is designed to execute compiled code efficiently. It manages execution contexts, handles exceptions, and maintains the runtime environment, ensuring accurate and smooth program execution.

2. **Scope Management**: Scopes are managed using nested structures (`CompilerState`) to track local variables and their lifetimes. This allows for correct variable resolution and promotion to upvalues when necessary.

3. **Intermediate Representation (IR)**: Chunks are used to represent IR, which is then emitted by the compiler. This separation of concerns helps in organizing the code and makes it easier to manage and optimize.

## Major Classes/Functions Overview

### Compiler Class

- **Purpose**: Manages the overall compilation process, including state tracking and chunk emission.
- **Key Functions**:
  - `compile(ASTNode &root)`: Compiles an entire program starting from the root AST node and returns the top-level Chunk.
  - `emit(Op op, int32_t operand = 0, int line = 0)`: Emits an opcode with an optional operand and line number.
  - `addConst(QuantumValue v)`, `addStr(const std::string &s)`: Adds constants and strings to the chunk.

### CompilerState Struct

- **Purpose**: Represents the current state of the compiler, including the chunk being generated, local variables, upvalues, and scope depth.
- **Key Members**:
  - `chunk`: A shared pointer to the current Chunk.
  - `locals`: A vector of Local structs representing local variables.
  - `upvalues`: A vector of UpvalueDesc structs representing upvalues.
  - `scopeDepth`: Tracks the current scope depth.
  - `enclosing`: Points to the enclosing CompilerState for nested scopes.

### Local Struct

- **Purpose**: Describes a local variable, including its name, depth, and whether it is captured as an upvalue.
- **Key Members**:
  - `name`: The name of the local variable.
  - `depth`: The scope depth at which the variable is declared.
  - `isCaptured`: Indicates whether the variable has been promoted to an upvalue.

### UpvalueDesc Struct

- **Purpose**: Describes an upvalue, indicating whether it is captured from a local or outer scope and its slot index.
- **Key Members**:
  - `isLocal`: Boolean flag indicating if the upvalue is captured from a local scope.
  - `index`: Slot index of the upvalue, either in the local scope or the outer scope.

### Helper Functions

- **Scope Management**:
  - `beginScope()`, `endScope(int line = 0)`: Manage the beginning and end of scopes, updating the scope depth and local variables.
  - `resolveLocal(CompilerState *state, const std::string &name)`, `resolveUpvalue(CompilerState *state, const std::string &name)`: Resolve local and upvalue references based on the current scope.
  - `addUpvalue(CompilerState *state, int index, bool isLocal)`: Add an upvalue to the current state.
  - `declareLocal(const std::string &name, int line = 0)`: Declare a new local variable.

- **Node Compilation**:
  - `compileNode(ASTNode &node)`, `compileBlock(BlockStmt &b)`, `compileExpr(ASTNode &node)`: Recursively compile different types of AST nodes.
  - `compileVarDecl(VarDecl &s, int line)`, `compileFunctionDecl(FunctionDecl &s, int line)`, `compileClassDecl(ClassDecl &s, int line)`, `compileIf(IfStmt &s, int line)`: Specialized functions for compiling specific types of statements like variable declarations, function definitions, class declarations, and conditional statements.

## Tradeoffs

1. **Complexity vs. Performance**: Managing nested scopes and upvalues adds complexity to the compiler but improves performance by allowing efficient variable resolution and promotion.

2. **Memory Usage vs. Execution Time**: Storing intermediate representations in memory can increase memory usage, but it reduces the need for repeated computations and increases execution time efficiency.

3. **Flexibility vs. Simplicity**: Allowing for dynamic scope resolution and upvalue promotion provides flexibility but can complicate the implementation and debugging process.

By understanding these components and their roles, developers can better appreciate how the QuantumLanguage compiler processes source code and optimizes the resulting executable chunks.