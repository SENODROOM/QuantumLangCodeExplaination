# QuantumLanguage Compiler - AST.h

## Overview

The `include/AST.h` header file is an essential part of the QuantumLanguage compiler, focusing on defining the Abstract Syntax Tree (AST) nodes used to represent the structure of the source code. This file plays a pivotal role in the compiler's parsing phase, where it helps in converting the textual representation of the program into a hierarchical tree-like structure. The AST serves as the foundation for subsequent phases, including semantic analysis, optimization, and code generation.

## Key Design Decisions

### Use of `std::variant` for Expressions

To handle multiple types of expressions flexibly without resorting to unions or inheritance, the compiler uses `std::variant`. This decision was made because `std::variant` provides a safer and more expressive way to manage different expression types compared to traditional union-based approaches. It ensures type safety at compile time and eliminates the need for manual type checking and casting.

### Separate Node Structures for Expressions and Statements

Separating node structures for expressions and statements enhances clarity and maintainability. Each category has its own set of rules and behaviors, making it easier to implement specific features and optimizations tailored to either expressions or statements. For example, variable declarations have additional attributes like whether they are constant (`isConst`) or pointers (`isPointer`), which would not apply to expressions.

## Documentation of Major Classes/Functions

### `NumberLiteral`

**Purpose:** Represents a numeric literal in the source code.
**Behavior:** Holds a double value representing the number.

### `StringLiteral`

**Purpose:** Represents a string literal in the source code.
**Behavior:** Holds a `std::string` value representing the text.

### `BoolLiteral`

**Purpose:** Represents a boolean literal in the source code.
**Behavior:** Holds a boolean value indicating `true` or `false`.

### `NilLiteral`

**Purpose:** Represents a nil or null literal in the source code.
**Behavior:** No properties; indicates the absence of a value.

### `Identifier`

**Purpose:** Represents an identifier (variable, function, etc.) in the source code.
**Behavior:** Holds a `std::string` name representing the identifier.

### `BinaryExpr`

**Purpose:** Represents a binary expression (e.g., `a + b`).
**Behavior:** Holds an operation (`op`), a left operand (`left`), and a right operand (`right`). Supports various arithmetic, logical, and bitwise operations.

### `UnaryExpr`

**Purpose:** Represents a unary expression (e.g., `-a`, `!b`).
**Behavior:** Holds an operation (`op`) and an operand (`operand`). Supports negation, inversion, and other single-operand operations.

### `AssignExpr`

**Purpose:** Represents an assignment expression (e.g., `x = y`, `z += 1`).
**Behavior:** Holds an operation (`op`), a target variable (`target`), and a value to assign (`value`). Supports different forms of assignment like simple assignment, compound assignments, and pointer assignments.

### `CallExpr`

**Purpose:** Represents a function call expression (e.g., `f(a, b)`).
**Behavior:** Holds a callee (`callee`) and a vector of arguments (`args`). Used to model function calls throughout the AST.

### `IndexExpr`

**Purpose:** Represents an indexed expression (e.g., `arr[i]`).
**Behavior:** Holds an object (`object`) and an index (`index`). Used to model array indexing and dictionary lookups.

### `SliceExpr`

**Purpose:** Represents a slicing expression (e.g., `arr[start:stop:step]`).
**Behavior:** Holds an object (`object`), optional start (`start`), stop (`stop`), and step (`step`) indices. Allows for flexible slicing syntax, accommodating cases where some parts might be omitted.

### `MemberExpr`

**Purpose:** Represents a member access expression (e.g., `obj.member`).
**Behavior:** Holds an object (`object`) and a member name (`member`). Used to model attribute access in objects.

### `ArrayLiteral`

**Purpose:** Represents an array literal in the source code.
**Behavior:** Holds a vector of elements (`elements`). Used to construct arrays directly in the AST.

### `DictLiteral`

**Purpose:** Represents a dictionary literal in the source code.
**Behavior:** Holds a vector of key-value pairs (`pairs`). Used to construct dictionaries directly in the AST.

### `LambdaExpr`

**Purpose:** Represents a lambda expression in the source code.
**Behavior:** Holds parameters (`params`), parameter types (`paramTypes`), default arguments (`defaultArgs`), return type (`returnType`), and a body (`body`). Used to define anonymous functions within the AST.

### `TernaryExpr`

**Purpose:** Represents a ternary conditional expression (e.g., `condition ? then : else`).
**Behavior:** Holds a condition (`condition`), a true branch (`thenExpr`), and a false branch (`elseExpr`). Used to model conditional logic in the AST.

### `SuperExpr`

**Purpose:** Represents a super expression (e.g., `super()` or `super.method()`).
**Behavior:** Holds an optional method name (`method`). If `method` is empty, it represents a call to the superclass constructor.

### `AddressOfExpr`

**Purpose:** Represents an address-of expression (e.g., `&var`).
**Behavior:** Holds an operand (`operand`). Used to model taking the address of variables in the AST.

### `DerefExpr`

**Purpose:** Represents a dereference expression (e.g., `*ptr`).
**Behavior:** Holds an operand (`operand`). Used to model accessing the value pointed to by a pointer in the AST.

### `ArrowExpr`

**Purpose:** Represents an arrow expression (e.g., `ptr->member`).
**Behavior:**