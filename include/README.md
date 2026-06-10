# QuantumLanguage Compiler - AST.h

## Overview

The `include/AST.h` header file in the QuantumLanguage compiler defines the Abstract Syntax Tree (AST) structure. The AST represents the syntactic structure of source code written in the QuantumLanguage, broken down into nodes that correspond to language constructs like expressions, statements, and declarations. This file serves as the foundation for parsing, analyzing, and transforming the source code into executable form.

## Role in Compiler Pipeline

The AST plays a pivotal role in the compiler's pipeline:

1. **Parsing**: The source code is parsed into an AST, which is a hierarchical representation of the code's syntax.
2. **Analysis**: The AST is analyzed to ensure correctness, identify potential errors, and optimize the code.
3. **Transformation**: The AST is transformed into a lower-level representation suitable for further processing, such as generating machine code or bytecode.
4. **Code Generation**: Finally, the AST is used to generate the target code, whether it be native machine code or bytecode for an intermediate virtual machine.

By using an AST, the compiler can handle complex language features and perform various optimizations more effectively than with a linear token stream.

## Key Design Decisions and Why

### Use of Variants

The AST uses `std::variant` to represent different types of expression and statement nodes. This choice allows for a flexible and extensible design where new node types can be easily added without modifying existing code.

```cpp
using ASTNodePtr = std::unique_ptr<ASTNode>;
```

### Hierarchical Structure

Each node type in the AST has a clear hierarchy, making it easier to traverse and manipulate the tree. For example, `BinaryExpr` contains pointers to its left and right operands, forming a binary tree structure.

```cpp
struct BinaryExpr {
    std::string op;
    ASTNodePtr left, right;
};
```

### Type Safety

Using smart pointers (`std::unique_ptr`) ensures that memory management is safe and efficient. Each node is owned by a unique pointer, preventing multiple ownership and ensuring that nodes are properly deallocated when they are no longer needed.

```cpp
struct ASTNode {
    virtual ~ASTNode() {}
};
```

## Major Classes/Functions Overview

### Expression Nodes

The AST includes various types of expression nodes:

- **NumberLiteral**: Represents numeric literals.
- **StringLiteral**: Represents string literals.
- **BoolLiteral**: Represents boolean literals.
- **NilLiteral**: Represents the nil literal.
- **Identifier**: Represents variable identifiers.
- **BinaryExpr**: Represents binary operations.
- **UnaryExpr**: Represents unary operations.
- **AssignExpr**: Represents assignment operations.
- **CallExpr**: Represents function calls.
- **IndexExpr**: Represents indexing operations.
- **SliceExpr**: Represents slicing operations similar to Python.
- **MemberExpr**: Represents member access.
- **ArrayLiteral**: Represents array literals.
- **DictLiteral**: Represents dictionary literals.
- **LambdaExpr**: Represents lambda functions.
- **TernaryExpr**: Represents ternary conditional expressions.
- **SuperExpr**: Represents super constructor or method calls.

### Statement Nodes

Similarly, there are various types of statement nodes:

- **VarDecl**: Represents variable declarations.
- **FunctionDecl**: Represents function declarations.
- **ReturnStmt**: Represents return statements.
- **IfStmt**: Represents if statements.
- **ForStmt**: Represents for loops.
- **WhileStmt**: Represents while loops.
- **BlockStmt**: Represents blocks of statements.

## Tradeoffs

### Memory Management

One tradeoff is the increased complexity of memory management due to the use of smart pointers. While this prevents common memory errors, it adds overhead compared to manual memory management.

### Flexibility vs. Complexity

The use of `std::variant` provides flexibility in representing different node types but increases the complexity of the AST traversal logic. Balancing these factors is essential for maintainability and performance.

### Performance vs. Debugging

The hierarchical structure of the AST makes it easier to debug and understand the code's structure. However, traversing the AST can be slower than working directly with a flat token stream, which could impact performance.

Overall, the AST.h file is a critical component of the QuantumLanguage compiler, providing a robust and flexible representation of the source code that supports various stages of compilation.