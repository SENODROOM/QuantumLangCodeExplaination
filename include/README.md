# QuantumLanguage Compiler - AST.h

## Overview

The `include/AST.h` header file is an essential part of the QuantumLanguage compiler, dedicated to defining the Abstract Syntax Tree (AST). The AST represents the syntactic structure of the source code in a tree-like format, making it easier to analyze, transform, and execute.

## Role in Compiler Pipeline

The AST serves as a crucial intermediate representation between the source code and the final executable. It is built during the parsing phase of the compilation process, where the source code is converted into a structured form that reflects its syntax. This structured data is then used by various stages of the compiler, including semantic analysis, optimization, and code generation.

### Key Design Decisions and Why

1. **Hierarchical Structure**: The AST uses a hierarchical structure with nodes representing different parts of the code. This allows for a clear and organized representation of the code's syntax.
2. **Flexibility**: By using variants (`std::variant`) and unique pointers (`std::unique_ptr`), the AST can represent a wide range of expressions and statements without requiring fixed types. This flexibility is essential for supporting multiple programming paradigms and language features.
3. **Memory Management**: Using unique pointers ensures proper memory management, preventing memory leaks and dangling pointers. This is particularly important in a compiler where large amounts of memory can be allocated during the parsing and AST construction phases.

## Major Classes/Functions Overview

### Expression Types

- **NumberLiteral**: Represents numeric literals in the code.
- **StringLiteral**: Represents string literals.
- **BoolLiteral**: Represents boolean literals.
- **NilLiteral**: Represents the nil literal.
- **Identifier**: Represents variable identifiers.
- **BinaryExpr**: Represents binary operations like addition, subtraction, etc.
- **UnaryExpr**: Represents unary operations like negation, increment, etc.
- **AssignExpr**: Represents assignment operations.
- **CallExpr**: Represents function calls.
- **IndexExpr**: Represents indexing operations.
- **SliceExpr**: Represents slicing operations in Python-like syntax.
- **MemberExpr**: Represents member access operations.
- **ArrayLiteral**: Represents array literals.
- **DictLiteral**: Represents dictionary literals.
- **LambdaExpr**: Represents lambda functions.
- **TernaryExpr**: Represents ternary conditional expressions.
- **SuperExpr**: Represents super constructor or method calls.

### C++ Pointer Expression Types

- **AddressOfExpr**: Represents the address-of operator (`&`).
- **DerefExpr**: Represents the dereference operator (`*`).
- **ArrowExpr**: Represents the arrow operator (`->`), which is used for accessing members of objects through pointers.

### Statement Types

- **VarDecl**: Represents variable declarations.
- **FunctionDecl**: Represents function declarations, including parameters, return types, and bodies.
- **ReturnStmt**: Represents return statements.
- **IfStmt**: Represents if statements, including conditions and branches.

## Tradeoffs

1. **Complexity vs. Flexibility**: While the use of variants and unique pointers provides great flexibility, it also increases the complexity of the AST implementation. Developers need to handle these data structures carefully to avoid errors and ensure correct behavior.
2. **Performance vs. Memory Usage**: The hierarchical structure of the AST requires additional memory to store the nodes and their relationships. However, this tradeoff is often acceptable because the benefits of having a well-structured AST outweigh the increased memory usage.
3. **Readability vs. Maintainability**: The use of forward declarations and nested structs can make the code harder to read and maintain. However, this approach helps keep the headers clean and focused on the AST definition, reducing clutter in other files.

By understanding these key components and considerations, developers can better appreciate the importance of the AST in the QuantumLanguage compiler and how it contributes to the overall efficiency and accuracy of the compilation process.