# Quantum Language Compiler - Abstract Syntax Tree (AST)

## Overview

The `include/AST.h` file defines the structure of the Abstract Syntax Tree (AST) used by the Quantum Language compiler. This file serves as the backbone for representing the syntax of the programming language in a structured manner, which facilitates semantic analysis, optimization, and code generation phases of the compilation process.

## Key Design Decisions

### Use of `std::variant` for Expressions

**Why:** To provide a flexible and extensible way to represent different types of expressions without using multiple inheritance or complex visitor patterns. This allows for easy addition of new expression types in the future without breaking existing code.

### Smart Pointers (`std::unique_ptr`) for AST Nodes

**Why:** To manage memory automatically and avoid manual memory management errors such as leaks or dangling pointers. Using smart pointers ensures that each node in the AST is properly deallocated when it is no longer needed, simplifying the overall memory management strategy of the compiler.

### Separate Classes for Expressions and Statements

**Why:** To clearly distinguish between syntactic constructs that evaluate to values (expressions) and those that perform actions (statements). This separation helps in organizing the codebase better and makes it easier to implement specific optimizations and transformations on either category.

## Class Documentation

### `NumberLiteral`

**Purpose:** Represents a numeric literal in the source code.

**Behavior:** Contains a `double` value that holds the numeric literal's value.

### `StringLiteral`

**Purpose:** Represents a string literal in the source code.

**Behavior:** Contains a `std::string` value that holds the string literal's content.

### `BoolLiteral`

**Purpose:** Represents a boolean literal in the source code.

**Behavior:** Contains a `bool` value that indicates whether the literal is `true` or `false`.

### `NilLiteral`

**Purpose:** Represents a `nil` literal in the source code, often used to denote the absence of a value.

**Behavior:** No data members, as it represents an empty state.

### `Identifier`

**Purpose:** Represents an identifier (variable, function, etc.) in the source code.

**Behavior:** Contains a `std::string` that holds the identifier's name.

### `BinaryExpr`

**Purpose:** Represents a binary expression (e.g., `a + b`).

**Behavior:** Contains an operation (`op`), a left-hand side (`left`), and a right-hand side (`right`). The operation is represented as a `std::string`, and both sides are pointers to `ASTNode`.

### `UnaryExpr`

**Purpose:** Represents a unary expression (e.g., `-a`).

**Behavior:** Contains an operation (`op`) and an operand (`operand`). The operation is represented as a `std::string`, and the operand is a pointer to `ASTNode`.

### `AssignExpr`

**Purpose:** Represents an assignment expression (e.g., `x = y`).

**Behavior:** Contains an operation (`op`), a target (`target`), and a value (`value`). The operation can be one of several assignment operators, and both the target and value are pointers to `ASTNode`.

### `CallExpr`

**Purpose:** Represents a function call expression (e.g., `func(arg)`).

**Behavior:** Contains a callee (`callee`) and a vector of arguments (`args`). Both the callee and arguments are pointers to `ASTNode`.

### `IndexExpr`

**Purpose:** Represents an indexing expression (e.g., `array[index]`).

**Behavior:** Contains an object (`object`) and an index (`index`). Both are pointers to `ASTNode`.

### `SliceExpr`

**Purpose:** Represents a slicing expression similar to Python's `obj[start:stop:step]`.

**Behavior:** Contains an object (`object`), optional start, stop, and step indices (`start`, `stop`, `step`). All indices are pointers to `ASTNode`. If an index is omitted, it defaults to `nullptr` (interpreted as `0`, `end`, or `1` respectively).

### `MemberExpr`

**Purpose:** Represents a member access expression (e.g., `obj.member`).

**Behavior:** Contains an object (`object`) and a member name (`member`). Both are pointers to `ASTNode`.

### `ArrayLiteral`

**Purpose:** Represents an array literal (e.g., `[1, 2, 3]`).

**Behavior:** Contains a vector of elements (`elements`). Each element is a pointer to `ASTNode`.

### `DictLiteral`

**Purpose:** Represents a dictionary literal (e.g., `{key: value}`).

**Behavior:** Contains a vector of key-value pairs (`pairs`). Each pair consists of two pointers to `ASTNode` representing the key and value respectively.

### `LambdaExpr`

**Purpose:** Represents a lambda function expression (e.g., `lambda x: x + 1`).

**Behavior:** Contains parameters (`params`), parameter types (`paramTypes`), default arguments (`defaultArgs`), return type (`returnType`), and a body (`body`). Parameters and their types are stored as vectors of strings, default arguments as pointers to `ASTNode`, and the return type as a string.

### `TernaryExpr`

**Purpose:** Represents a ternary conditional expression (e.g., `condition ? then : else`).

**Behavior:** Contains a condition (`condition`), a true branch (`thenExpr`), and a false branch (`elseExpr`). All branches are pointers to `ASTNode`.

### `SuperExpr`

**Purpose:** Represents a `super()` call or a method call on the superclass.

**Behavior:** Contains an optional method name (`method`). If `method` is empty, it represents a call to the superclass constructor. Otherwise, it represents a method call on the superclass.

## Tradeoffs and Limitations