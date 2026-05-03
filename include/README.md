# QuantumLanguage Compiler - Compiler.h

## Overview

The `include/Compiler.h` header file is a central part of the QuantumLanguage compiler, focusing on defining the core components necessary for compiling quantum programs into executable bytecode. The primary responsibility of this file is to manage the compilation process, including parsing abstract syntax trees (ASTs), generating intermediate representation (IR), and emitting low-level instructions that can be executed by the virtual machine (VM).

## Role in Compiler Pipeline

The `Compiler.h` file plays a crucial role in the overall compilation pipeline of the QuantumLanguage compiler. It starts with the parsing phase where it takes the source code and converts it into an AST. Then, it proceeds to the IR generation phase, where it translates the AST into a more structured form suitable for further processing. Finally, it emits the low-level instructions that will be executed by the VM.

## Key Design Decisions and Why

1. **Virtual Machine (VM)**: The design decision to use a VM was made to provide a clear separation between the compilation process and the execution environment. This allows for easier maintenance and optimization of both phases independently.

2. **Execution Context Management**: The compiler maintains an execution context through the `CompilerState` class. Each `CompilerState` represents a different scope within the program, allowing the compiler to track variables and their lifetimes accurately.

3. **Intermediate Representation (IR)**: By using an IR, the compiler can perform optimizations before emitting the final bytecode. This approach helps in reducing the complexity of the emitted code and improving performance.

4. **Dynamic Memory Allocation**: The use of smart pointers (`std::shared_ptr`) ensures that memory is managed dynamically and efficiently, preventing memory leaks and ensuring proper resource cleanup.

5. **Error Handling**: The compiler includes mechanisms for handling errors and exceptions, such as tracking the current line number and providing detailed error messages. This helps in debugging and maintaining robustness in the compilation process.

## Major Classes/Functions Overview

### Compiler Class

- **Purpose**: Manages the entire compilation process.
- **Key Functions**:
  - `compile(ASTNode &root)`: Compiles the entire program starting from the root node of the AST.
  - `compileNode(ASTNode &node)`, `compileBlock(BlockStmt &b)`, `compileExpr(ASTNode &node)`: Recursively compiles different types of nodes in the AST.
  - `compileVarDecl(VarDecl &s, int line)`, `compileFunctionDecl(FunctionDecl &s, int line)`, `compileClassDecl(ClassDecl &s, int line)`, `compileIf(IfStmt &s, int line)`: Specialized functions for compiling specific types of statements.

### CompilerState Struct

- **Purpose**: Represents the current state of the compiler, including the chunk being generated, local variables, upvalues, and scope depth.
- **Key Members**:
  - `chunk`: A shared pointer to the current chunk being compiled.
  - `locals`: A vector of local variables within the current scope.
  - `upvalues`: A vector of upvalues used to capture variables from outer scopes.
  - `scopeDepth`: Tracks the current depth of the scope.
  - `enclosing`: Points to the enclosing compiler state, forming a stack-like structure for nested scopes.
  - `isFunction`: Indicates whether the current scope is a function.

### Helper Functions

- **emit(Op op, int32_t operand = 0, int line = 0)**: Emits an opcode along with an optional operand and line number to the current chunk.
- **addConst(QuantumValue v)**, **addStr(const std::string &s)**: Adds constants and strings to the chunk and returns their indices.
- **emitJump(Op op, int line = 0)**, **patchJump(size_t idx)**: Handles jump instructions, emitting placeholders and patching them later with the correct offsets.

## Tradeoffs

1. **Complexity vs. Performance**: Using an IR adds complexity to the compiler but can lead to better performance in the emitted bytecode due to optimizations.

2. **Memory Management**: Dynamic memory allocation with smart pointers provides flexibility but may increase overhead compared to manual memory management.

3. **Flexibility vs. Simplicity**: The modular design of the compiler allows for easy extension and modification but may result in a more complex codebase.

Overall, the `Compiler.h` file is essential for managing the compilation process of QuantumLanguage programs, balancing complexity, performance, and maintainability.