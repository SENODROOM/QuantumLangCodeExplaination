# QuantumLanguage Compiler - Compiler.h

The `include/Compiler.h` header file is a critical component of the QuantumLanguage compiler, responsible for compiling abstract syntax trees (ASTs) into executable chunks. The primary focus of this file is on the `Compiler` class, which manages the compilation process, including scope management, opcode emission, and constant addition.

## Role in Compiler Pipeline

The `Compiler` class operates within the compiler pipeline, specifically during the translation phase where AST nodes are transformed into intermediate representation (IR). It ensures that each node is correctly translated into corresponding IR instructions, taking care of variable declarations, function definitions, control structures, and other language constructs.

## Key Design Decisions and WHY

### Scope Management

- **Why**: To manage variables and their lifetimes effectively, allowing for nested scopes and capturing local variables in closures.
- **Implementation**: The `Compiler` uses nested `CompilerState` structs to represent different scopes. Each `CompilerState` contains information about local variables, upvalues, and the current scope depth.

### Opcode Emission

- **Why**: To generate low-level instructions that can be executed by the virtual machine (VM).
- **Implementation**: The `emit` function adds new instructions to the current chunk. It supports various opcodes such as `OP_ADD`, `OP_SUBTRACT`, `OP_JUMP`, etc., and allows for optional operands and line numbers for debugging purposes.

### Constant Addition

- **Why**: To store constants like integers, strings, and objects in the chunk, making them accessible during execution.
- **Implementation**: The `addConst` and `addStr` functions add quantum values and strings to the chunk's constant pool, respectively. They return indices that can be used to reference these constants in emitted opcodes.

## Major Classes/Functions Overview

### Compiler Class

- **Overview**: Manages the compilation of an entire program by traversing the AST and emitting corresponding IR instructions.
- **Key Functions**:
  - `compile(ASTNode &root)`: Compiles the root AST node and returns the top-level chunk.
  - `compileNode(ASTNode &node)`: Recursively compiles an AST node based on its type.
  - `compileBlock(BlockStmt &b)`: Compiles a block statement, managing scope and control flow.
  - `compileExpr(ASTNode &node)`: Compiles an expression node, generating the necessary IR instructions.

### CompilerState Struct

- **Overview**: Represents the state of the compiler at a particular point in time, including the current chunk, local variables, upvalues, and scope depth.
- **Key Members**:
  - `chunk`: Shared pointer to the current chunk being compiled.
  - `locals`: Vector of local variables declared in the current scope.
  - `upvalues`: Vector of upvalues used to capture local variables from outer scopes.
  - `scopeDepth`: Current depth of the scope.
  - `enclosing`: Pointer to the enclosing compiler state, forming a stack-like structure for nested scopes.

### Scope Management Functions

- **beginScope()**: Marks the beginning of a new scope.
- **endScope(int line)**: Ends the current scope, resolving any remaining local variables.
- **resolveLocal(CompilerState *state, const std::string &name)**: Resolves a local variable by searching the current scope and its enclosing scopes.
- **resolveUpvalue(CompilerState *state, const std::string &name)**: Resolves an upvalue by creating a new upvalue descriptor if it doesn't already exist.
- **addUpvalue(CompilerState *state, int index, bool isLocal)**: Adds a new upvalue to the current state.
- **declareLocal(const std::string &name, int line)**: Declares a new local variable in the current scope.

## Tradeoffs

- **Memory Usage**: Using shared pointers for chunks and states minimizes memory duplication but increases overhead due to reference counting.
- **Performance**: Efficiently managing scope and upvalues improves performance by reducing the need for dynamic memory allocation and lookup times.
- **Debugging**: Storing line numbers with each opcode facilitates easier debugging and error reporting.

Overall, the `Compiler.h` file provides the core functionality for translating QuantumLanguage programs into executable IR, ensuring efficient and accurate compilation while managing complex aspects of scope and upvalues.