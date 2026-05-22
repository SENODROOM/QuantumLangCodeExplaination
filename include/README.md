# QuantumLanguage Compiler - AST.h

## Overview

The `include/AST.h` header file is a crucial component of the QuantumLanguage compiler, focused on defining the Abstract Syntax Tree (AST) structure. This AST represents the syntactic structure of the source code in a tree-like format, which is used during the compilation process to analyze and transform the code into executable instructions.

## Role in Compiler Pipeline

The AST serves as the foundation for various stages of the compiler pipeline:

1. **Parsing**: Converts the source code into an AST.
2. **Semantic Analysis**: Validates the AST against language rules and resolves symbols.
3. **Code Generation**: Translates the AST into target machine code.
4. **Optimization**: Improves the efficiency of the generated code.

By representing the source code as an AST, the compiler can perform complex transformations and optimizations that would be difficult or impossible with a linear representation of the code.

## Key Design Decisions and Why

### Use of Variants

The AST uses `std::variant` to represent different types of expressions and statements. This choice allows for a flexible and extensible representation, where new expression or statement types can be easily added without modifying existing code.

### Smart Pointers

All nodes in the AST are managed using `std::unique_ptr`, which ensures automatic memory management and prevents memory leaks. This approach aligns with modern C++ practices and enhances the safety and reliability of the compiler.

### Hierarchical Structure

The AST is designed with a hierarchical structure, where each node can have child nodes. This design facilitates easy traversal and manipulation of the tree, making it suitable for a wide range of operations during the compilation process.

## Major Classes/Functions Overview

### Expression Types

- **NumberLiteral**: Represents numeric literals.
- **StringLiteral**: Represents string literals.
- **BoolLiteral**: Represents boolean literals.
- **NilLiteral**: Represents the `nil` literal.
- **Identifier**: Represents variable identifiers.
- **BinaryExpr**: Represents binary operators like `+`, `-`, `*`, etc.
- **UnaryExpr**: Represents unary operators like `!`, `-`, etc.
- **AssignExpr**: Represents assignment operations like `=`, `+=`, `-=`, etc.
- **CallExpr**: Represents function calls.
- **IndexExpr**: Represents indexing operations.
- **SliceExpr**: Represents slicing operations similar to Python's `[start:stop:step]`.
- **MemberExpr**: Represents member access.
- **ArrayLiteral**: Represents array literals.
- **DictLiteral**: Represents dictionary literals.
- **LambdaExpr**: Represents lambda functions.
- **TernaryExpr**: Represents ternary conditional expressions.
- **SuperExpr**: Represents calls to superclass constructors or methods.

### C++ Pointer Expression Types

- **AddressOfExpr**: Represents the address-of operator (`&`).
- **DerefExpr**: Represents the dereference operator (`*`).
- **ArrowExpr**: Represents member access through pointers (`->`).

### Statement Types

- **VarDecl**: Represents variable declarations.
- **FunctionDecl**: Represents function declarations.
- **ReturnStmt**: Represents return statements.
- **IfStmt**: Represents conditional statements.

## Tradeoffs

### Flexibility vs. Complexity

Using `std::variant` provides flexibility in adding new expression or statement types, but it also increases the complexity of the AST implementation. Developers must be careful to handle all possible variants when traversing or manipulating the AST.

### Memory Management

Smart pointers (`std::unique_ptr`) simplify memory management by automatically deallocating memory when nodes are no longer needed. However, they introduce additional overhead compared to raw pointers, which might impact performance in some critical sections of the compiler.

### Hierarchical Structure vs. Flat Representation

A hierarchical AST offers a more intuitive and structured representation of the code, making it easier to understand and manipulate. On the other hand, a flat representation could potentially reduce complexity, although it would make certain operations more cumbersome.

Overall, the design of the AST in `include/AST.h` aims to balance flexibility, simplicity, and performance, providing a robust foundation for the QuantumLanguage compiler's various stages.