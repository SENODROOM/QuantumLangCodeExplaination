# QuantumLanguage Compiler - AST.h

## Overview

The `include/AST.h` header file is central to the QuantumLanguage compiler, defining the Abstract Syntax Tree (AST) structure. The AST represents the syntactic structure of source code written in the QuantumLanguage, breaking it down into a tree-like structure where each node corresponds to a construct in the language such as expressions, statements, and declarations. This representation allows the compiler to perform semantic analysis, optimization, and code generation more effectively.

## Role in Compiler Pipeline

The AST serves as an intermediate representation between the source code and the final executable. It is built during the parsing phase, where the compiler transforms the raw text into a structured format. The AST is then used throughout the rest of the compilation process:

1. **Semantic Analysis**: Ensures that the code adheres to the language's rules and constraints.
2. **Optimization**: Identifies opportunities to improve performance without changing the semantics.
3. **Code Generation**: Translates the AST into machine code or another lower-level representation suitable for execution.

## Key Design Decisions and Why

### Node Variants

The AST is designed using `std::variant`, allowing for multiple types of nodes within the same container. This flexibility is essential because different constructs in the language can have vastly different structures and behaviors.

### Expression Types

Several expression types are defined, including:
- **NumberLiteral**: Represents numeric literals like integers and floating-point numbers.
- **StringLiteral**: Represents string literals.
- **BoolLiteral** and **NilLiteral**: Represent boolean and nil values respectively.
- **Identifier**: Refers to variable names or function identifiers.
- **BinaryExpr**, **UnaryExpr**, and **AssignExpr**: Handle binary operations, unary operations, and assignment expressions.
- **CallExpr**, **IndexExpr**, **SliceExpr**, **MemberExpr**: Manage function calls, array indexing, slicing, and member access.
- **ArrayLiteral** and **DictLiteral**: Represent literal arrays and dictionaries.
- **LambdaExpr**: Defines anonymous functions with parameters, return types, and bodies.
- **TernaryExpr**: Handles conditional expressions similar to the ternary operator in other languages.
- **SuperExpr**: Allows calling superclass constructors or methods.

### C++ Pointer Expression Types

Additional expression types are provided for handling pointers in C++, including:
- **AddressOfExpr**: Represents the address-of operator (`&`).
- **DerefExpr**: Represents the dereference operator (`*`).
- **ArrowExpr**: Represents member access through pointers (`ptr->member`).

### Statement Types

Statement types include:
- **VarDecl**: Declares variables with optional initializers and type hints.
- **FunctionDecl**: Defines functions with parameter lists, return types, and bodies.
- **ReturnStmt**: Handles return statements with optional values.
- **IfStmt**: Manages conditional blocks with conditions and branches.

## Major Classes/Functions Overview

### ASTNode

The base class for all AST nodes. It uses `std::variant` to store different types of nodes.

### Expression Classes

Classes representing various expression types such as `NumberLiteral`, `StringLiteral`, `BinaryExpr`, etc. Each class encapsulates the specific data and behavior associated with its type.

### Statement Classes

Classes representing different statement types such as `VarDecl`, `FunctionDecl`, `ReturnStmt`, and `IfStmt`. These classes manage the syntax and semantics of their respective constructs.

### Helper Functions

Helper functions are provided to create and manipulate AST nodes, ensuring that the AST remains consistent and well-formed.

## Tradeoffs

### Flexibility vs. Complexity

Using `std::variant` provides significant flexibility but also increases complexity in terms of type checking and pattern matching. Balancing these factors was crucial for creating a robust yet maintainable AST system.

### Memory Management

The use of smart pointers (`std::unique_ptr`) for managing AST nodes helps prevent memory leaks and dangling references. However, it adds overhead compared to manual memory management.

### Performance vs. Usability

While the AST offers powerful features for advanced optimizations and analyses, it can be challenging to work with due to its complexity. Finding a balance between performance and usability was one of the primary goals in designing the AST.

Overall, the `include/AST.h` header file is a critical component of the QuantumLanguage compiler, providing a structured and flexible way to represent source code and facilitating efficient compilation processes.