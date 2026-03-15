# QuantumLanguage Compiler - AST.h

## Overview

The `include/AST.h` header file is an integral component of the QuantumLanguage compiler, responsible for defining and managing the Abstract Syntax Tree (AST). The AST represents the syntactic structure of the source code as a tree-like graph, where each node corresponds to a construct in the language. This file plays a crucial role in the compiler pipeline by providing the foundational structures necessary for parsing, semantic analysis, and code generation phases.

## Key Design Decisions

### Use of `std::variant`
- **Why**: Leveraging `std::variant` allows for the encapsulation of multiple possible value types within a single unified type. This choice simplifies the handling of different data types in expressions and literals.
  
### Smart Pointers (`std::unique_ptr`)
- **Why**: Using smart pointers ensures automatic memory management and prevents memory leaks. It also enhances safety by ensuring exclusive ownership of resources, which is particularly important when dealing with dynamically allocated AST nodes.

## Class Documentation

### ASTNode
- **Purpose**: Base class for all AST nodes. It serves as a polymorphic container for different expression and statement types.
- **Behavior**: Provides virtual functions that derived classes must implement to define their specific behaviors.

### NumberLiteral
- **Purpose**: Represents numeric literals in the source code.
- **Behavior**: Holds a `double` value representing the literal number.

### StringLiteral
- **Purpose**: Represents string literals in the source code.
- **Behavior**: Holds a `std::string` value representing the literal string.

### BoolLiteral
- **Purpose**: Represents boolean literals in the source code.
- **Behavior**: Holds a `bool` value indicating whether the literal is `true` or `false`.

### NilLiteral
- **Purpose**: Represents the `nil` literal in the source code, often used to denote the absence of a value.
- **Behavior**: No additional members since it represents an empty state.

### Identifier
- **Purpose**: Represents variable identifiers in the source code.
- **Behavior**: Holds a `std::string` value representing the identifier's name.

### BinaryExpr
- **Purpose**: Represents binary expressions such as addition, subtraction, etc.
- **Behavior**: Contains an operation (`op`) and two operands (`left` and `right`). Each operand is an `ASTNodePtr`.

### UnaryExpr
- **Purpose**: Represents unary expressions such as negation, increment, etc.
- **Behavior**: Contains an operation (`op`) and one operand (`operand`). The operand is an `ASTNodePtr`.

### AssignExpr
- **Purpose**: Represents assignment expressions, including compound assignments.
- **Behavior**: Contains an operation (`op`, e.g., `=`), a target (`target`), and a value (`value`). Both the target and value are `ASTNodePtr`.

### CallExpr
- **Purpose**: Represents function calls.
- **Behavior**: Contains a callee (`callee`, an `ASTNodePtr`) and a vector of arguments (`args`).

### IndexExpr
- **Purpose**: Represents indexing operations on arrays or other indexed objects.
- **Behavior**: Contains an object (`object`, an `ASTNodePtr`) and an index (`index`, an `ASTNodePtr`).

### SliceExpr
- **Purpose**: Represents slicing operations similar to Python's `[start:stop:step]`.
- **Behavior**: Contains an object (`object`, an `ASTNodePtr`), optional start, stop, and step indices (`start`, `stop`, `step`, each being an `ASTNodePtr`).

### MemberExpr
- **Purpose**: Represents member access, either through dot notation or arrow notation.
- **Behavior**: Contains an object (`object`, an `ASTNodePtr`) and a member name (`member`, a `std::string`).

### ArrayLiteral
- **Purpose**: Represents array literals.
- **Behavior**: Contains a vector of elements (`elements`), each being an `ASTNodePtr`.

### DictLiteral
- **Purpose**: Represents dictionary literals.
- **Behavior**: Contains a vector of key-value pairs (`pairs`), where both keys and values are `ASTNodePtr`.

### LambdaExpr
- **Purpose**: Represents lambda expressions.
- **Behavior**: Contains parameters (`params`), parameter types (`paramTypes`), default arguments (`defaultArgs`), return type (`returnType`, a `std::string`), and a body (`body`, an `ASTNodePtr`).

### TernaryExpr
- **Purpose**: Represents ternary conditional expressions.
- **Behavior**: Contains a condition (`condition`, an `ASTNodePtr`), a true branch (`thenExpr`, an `ASTNodePtr`), and an else branch (`elseExpr`, an `ASTNodePtr`).

### SuperExpr
- **Purpose**: Represents super constructor or method calls.
- **Behavior**: Contains an optional method name (`method`, a `std::string`). If empty, it denotes a super constructor call.

## Tradeoffs and Limitations

- **Flexibility vs. Complexity**: While using `std::variant` simplifies the representation of different value types, it might introduce complexity in terms of type checking and handling during semantic analysis.
- **Memory Management**: Although smart pointers ensure automatic memory management, they might add overhead compared to raw pointers, especially in performance-critical sections of the compiler.
- **Extensibility**: Adding new expression or statement types requires modifications to the existing hierarchy, which could lead to increased coupling between components.

This README provides a comprehensive overview of the `AST.h` file, detailing its role, design choices, and the behavior of each major class/function.