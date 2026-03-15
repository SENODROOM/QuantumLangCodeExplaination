# QuantumLanguage Compiler - AST.h

## Overview

The `include/AST.h` header file is a fundamental component of the QuantumLanguage compiler, responsible for defining the Abstract Syntax Tree (AST) nodes that represent the structure of the language's programs. These AST nodes serve as the foundation for semantic analysis, code generation, and other phases of the compilation process.

This file includes definitions for various expression and statement types, which are essential for accurately representing the syntax of QuantumLanguage. By using these AST nodes, the compiler can maintain a clear understanding of the program's structure and semantics throughout the entire compilation pipeline.

## Key Design Decisions

### Use of `std::variant` for Expressions

One of the primary design decisions in `AST.h` is the use of `std::variant` to represent expressions. This approach allows for a flexible and extensible way to define different types of expressions without requiring multiple inheritance or complex visitor patterns. By encapsulating all possible expression types within a single variant, the compiler can easily work with and manipulate expressions at runtime.

**Why:** Using `std::variant` simplifies the implementation of the AST by reducing the overhead associated with multiple inheritance and making it easier to add new expression types in the future.

### Separate Visitor Pattern for Statements

Unlike expressions, statements do not directly inherit from a common base class. Instead, a separate visitor pattern is used to handle different statement types. This design choice ensures that each statement type has a well-defined interface for visiting, making it easier to implement and extend the visitor pattern.

**Why:** Separating the visitor pattern for statements into its own namespace allows for more modular and maintainable code. It also makes it easier to understand the purpose and behavior of each statement type, as they are isolated from one another.

## Documentation of Major Classes/Functions

### ASTNode

The `ASTNode` struct serves as the base class for all AST nodes. It contains a `type` field that indicates the type of the node, as well as a `location` field that stores information about the node's position in the source code.

**Purpose:** The `ASTNode` struct provides a common interface for all AST nodes, allowing them to be manipulated and traversed uniformly.

**Behavior:** When creating an AST node, you should set the `type` field to indicate the type of the node, and the `location` field to store information about the node's position in the source code.

### NumberLiteral

The `NumberLiteral` struct represents a numeric literal in the source code. It contains a `value` field that stores the numeric value represented by the literal.

**Purpose:** The `NumberLiteral` struct allows the compiler to represent numeric literals in the AST, enabling subsequent semantic analysis and code generation.

**Behavior:** When creating a `NumberLiteral`, you should set the `value` field to the desired numeric value.

### StringLiteral

The `StringLiteral` struct represents a string literal in the source code. It contains a `value` field that stores the string value represented by the literal.

**Purpose:** The `StringLiteral` struct allows the compiler to represent string literals in the AST, enabling subsequent semantic analysis and code generation.

**Behavior:** When creating a `StringLiteral`, you should set the `value` field to the desired string value.

### BoolLiteral

The `BoolLiteral` struct represents a boolean literal in the source code. It contains a `value` field that stores the boolean value represented by the literal.

**Purpose:** The `BoolLiteral` struct allows the compiler to represent boolean literals in the AST, enabling subsequent semantic analysis and code generation.

**Behavior:** When creating a `BoolLiteral`, you should set the `value` field to either `true` or `false`.

### NilLiteral

The `NilLiteral` struct represents a nil literal in the source code. It does not contain any fields, as it simply indicates the absence of a value.

**Purpose:** The `NilLiteral` struct allows the compiler to represent nil literals in the AST, enabling subsequent semantic analysis and code generation.

**Behavior:** When creating a `NilLiteral`, you should not set any fields, as it simply indicates the absence of a value.

### Identifier

The `Identifier` struct represents an identifier in the source code. It contains a `name` field that stores the name of the identifier.

**Purpose:** The `Identifier` struct allows the compiler to represent identifiers in the AST, enabling subsequent semantic analysis and code generation.

**Behavior:** When creating an `Identifier`, you should set the `name` field to the desired identifier name.

### BinaryExpr

The `BinaryExpr` struct represents a binary expression in the source code. It contains an `op` field that stores the operator of the expression, as well as `left` and `right` fields that store pointers to the operands of the expression.

**Purpose:** The `BinaryExpr` struct allows the compiler to represent binary expressions in the AST, enabling subsequent semantic analysis and code generation.

**Behavior:** When creating a `BinaryExpr`, you should set the `op` field to the desired operator, and the `left` and `right` fields to pointers to the operands of the expression.

### UnaryExpr

The `UnaryExpr` struct represents a unary expression in the source code. It contains an `op` field that stores the operator of the expression, as well as an `operand` field that stores a pointer to the operand of the expression.

**Purpose:** The `UnaryExpr` struct allows the compiler to represent unary expressions in the AST, enabling subsequent semantic analysis and code generation.

**Behavior:** When creating a `UnaryExpr`, you should set the `op` field to the desired operator, and the `operand` field to a pointer to the operand of the expression.

### AssignExpr

The `AssignExpr` struct represents an assignment expression in the source code. It contains an