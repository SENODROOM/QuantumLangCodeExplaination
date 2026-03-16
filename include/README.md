# QuantumLanguage Compiler - Compiler.h

## Overview

The `include/Compiler.h` header file is a critical component of the QuantumLanguage compiler, responsible for orchestrating the compilation process of the language's abstract syntax tree (AST). This file defines the primary `Compiler` class and several supporting structures that manage the compilation state, scopes, and upvalues. It ensures efficient handling of expressions, statements, and function calls, enabling the interpreter to execute quantum programs effectively.

## Role in Compiler Pipeline

The `Compiler.h` file plays a pivotal role in the QuantumLanguage compiler pipeline. It initializes the compilation process, manages the stack of compiler states, and handles the emission of bytecode instructions. The `compile` method serves as the entry point for compiling an entire program, starting with the root AST node and producing a top-level `Chunk` containing the compiled bytecode.

## Key Design Decisions and Why

### Use of Smart Pointers

Smart pointers (`std::shared_ptr`, `std::unique_ptr`) are extensively used throughout the `Compiler.h` file to manage memory safely and efficiently. This approach minimizes manual memory management overhead and helps prevent memory leaks, ensuring robustness and scalability of the compiler.

### Separate Compilation States

Each `CompilerState` instance represents a distinct scope in the source code, capturing details such as the current chunk of bytecode, local variables, upvalues, and scope depth. This separation allows for nested scoping and promotes readability and maintainability of the code.

### Efficient Symbol Resolution

The `resolveLocal` and `resolveUpvalue` methods provide efficient symbol resolution, reducing lookup times and improving performance. These methods utilize hash maps and vectors to store and quickly access information about local and upvalue variables, ensuring optimal symbol management.

## Major Classes/Functions Overview

### Compiler Class

- **Overview**: The main class responsible for driving the compilation process.
- **Methods**:
  - `compile(ASTNode &root)`: Compiles an entire program starting from the root AST node.
  - `beginScope()`, `endScope(int line)`: Manage the beginning and end of scopes, respectively.
  - `declareLocal(const std::string &name, int line)`: Declare a new local variable in the current scope.
  - `compileNode(ASTNode &node)`, `compileBlock(BlockStmt &b)`, `compileExpr(ASTNode &node)`: Recursively compile different types of AST nodes.
  - `compileVarDecl(VarDecl &s, int line)`, `compileFunctionDecl(FunctionDecl &s, int line)`, `compileClassDecl(ClassDecl &s, int line)`, `compileIf(IfStmt &s, int line)`: Specialized methods for compiling specific types of statements.

### UpvalueDesc Struct

- **Overview**: Describes an upvalue, which is a reference to a local variable from an outer function.
- **Members**:
  - `isLocal`: Indicates whether the upvalue is captured from a local variable.
  - `index`: Slot index for the upvalue.

### CompilerState Struct

- **Overview**: Captures the state of the compiler for each scope.
- **Members**:
  - `chunk`: Pointer to the current chunk of bytecode being generated.
  - `locals`: Vector of local variables in the current scope.
  - `upvalues`: Vector of upvalues in the current scope.
  - `scopeDepth`: Current depth of the scope.
  - `enclosing`: Pointer to the enclosing compiler state.

## Tradeoffs

### Memory Management

Using smart pointers introduces additional overhead compared to raw pointers but significantly reduces the risk of memory leaks and dangling pointers. The tradeoff is between increased runtime cost and improved safety and reliability.

### Complexity vs. Performance

The separation of compilation states and efficient symbol resolution enhance the clarity and maintainability of the code, but they may introduce some complexity. Balancing these aspects ensures that the compiler remains both readable and performant.

### Flexibility vs. Simplicity

The use of separate classes and functions for different parts of the compilation process offers high flexibility and modularity. However, it might increase the overall complexity of the system, requiring more careful management of dependencies and interactions.

In conclusion, the `include/Compiler.h` header file is a vital part of the QuantumLanguage compiler, providing essential functionality for managing compilation states, scopes, and upvalues. Its design choices balance memory safety, performance, and flexibility, making it a robust foundation for the language's interpreter.