# QuantumLanguage Compiler - AST.h

## Overview

The `include/AST.h` header file is an essential part of the QuantumLanguage compiler, focusing on defining and managing the Abstract Syntax Tree (AST). This file plays a pivotal role in the compiler pipeline by providing a structured representation of the source code, which facilitates subsequent phases like semantic analysis, code generation, and optimization. The AST is built dynamically during parsing and represents the syntactic structure of the program.

## Key Design Decisions

### Use of `std::variant` for Expressions

The decision to use `std::variant` instead of traditional inheritance-based approaches was driven by the need for flexibility and performance. `std::variant` allows for compile-time polymorphism without the overhead of virtual function calls, making it more efficient and easier to manage different expression types.

### Separate Structures for Expressions and Statements

Separating expressions and statements into distinct structures ensures clear separation of concerns and makes the codebase more modular and easier to understand. Each structure has a specific purpose and behavior, simplifying the implementation of the interpreter and other components.

### Support for Advanced Features

To cater to advanced programming features, such as lambda expressions, array literals, dictionary literals, and ternary operators, the AST design includes specialized structures. These structures provide a way to represent complex constructs in a straightforward manner, enhancing the expressive power of the language.

## Documentation of Major Classes/Functions

### ASTNode

**Purpose:** Base class for all AST nodes.

**Behavior:** Acts as a container for different types of AST nodes, providing common functionality and interfaces.

### NumberLiteral

**Purpose:** Represents numeric literals in the source code.

**Behavior:** Holds a double value representing the literal number.

### StringLiteral

**Purpose:** Represents string literals in the source code.

**Behavior:** Holds a std::string value representing the literal string.

### BoolLiteral

**Purpose:** Represents boolean literals in the source code.

**Behavior:** Holds a bool value indicating whether the literal is true or false.

### NilLiteral

**Purpose:** Represents the nil literal in the source code.

**Behavior:** No additional data, used to indicate the absence of a value.

### Identifier

**Purpose:** Represents variable identifiers in the source code.

**Behavior:** Holds a std::string value representing the identifier name.

### BinaryExpr

**Purpose:** Represents binary expressions (e.g., `a + b`).

**Behavior:** Contains an operation string (`op`) and pointers to the left and right operands (`left`, `right`). This structure allows for easy evaluation of binary operations during interpretation.

### UnaryExpr

**Purpose:** Represents unary expressions (e.g., `-a`).

**Behavior:** Contains an operation string (`op`) and a pointer to the operand (`operand`). This structure enables handling of unary operations efficiently.

### AssignExpr

**Purpose:** Represents assignment expressions (e.g., `x = y`).

**Behavior:** Contains an operation string (`op`), a pointer to the target variable (`target`), and a pointer to the assigned value (`value`). The operation string specifies the type of assignment (e.g., `=`).

### CallExpr

**Purpose:** Represents function call expressions (e.g., `f(x)`).

**Behavior:** Contains a pointer to the callee function (`callee`) and a vector of pointers to arguments (`args`). This structure facilitates the invocation of functions during interpretation.

### IndexExpr

**Purpose:** Represents indexing expressions (e.g., `arr[0]`).

**Behavior:** Contains a pointer to the object being indexed (`object`) and a pointer to the index (`index`). This structure supports accessing elements in arrays and other indexed data structures.

### SliceExpr

**Purpose:** Represents slicing expressions (e.g., `arr[start:stop:step]`).

**Behavior:** Contains a pointer to the object being sliced (`object`), optional pointers to the start, stop, and step indices (`start`, `stop`, `step`). If an index is omitted, it defaults to the beginning, end, or step size of 1, respectively.

### MemberExpr

**Purpose:** Represents member access expressions (e.g., `obj.member`).

**Behavior:** Contains a pointer to the object (`object`) and a std::string representing the member name (`member`). This structure supports accessing members of objects during interpretation.

### ArrayLiteral

**Purpose:** Represents array literals in the source code.

**Behavior:** Contains a vector of pointers to elements (`elements`). This structure allows for the creation and manipulation of arrays during interpretation.

### DictLiteral

**Purpose:** Represents dictionary literals in the source code.

**Behavior:** Contains a vector of key-value pairs (`pairs`), where each pair consists of a pointer to the key and a pointer to the value. This structure supports the creation and manipulation of dictionaries during interpretation.

### LambdaExpr

**Purpose:** Represents lambda expressions in the source code.

**Behavior:** Contains a vector of parameter names (`params`), a vector of parameter types (`paramTypes`), a vector of default argument values (`defaultArgs`), a return type string (`returnType`), and a pointer to the body of the lambda expression (`body`). This structure provides a way to define anonymous functions with explicit type hints.

### TernaryExpr

**Purpose:** Represents ternary conditional expressions (e.g., `condition ? then : else`).

**Behavior:** Contains a pointer to the condition (`condition`), a pointer to the expression executed if the condition is true (`thenExpr`), and a pointer to the expression executed if the condition is false (`elseExpr`). This structure supports conditional logic during interpretation.

### SuperExpr

**Purpose:** Represents super constructor or method calls.

**Behavior:** Contains an optional std::string representing the method name (`method`). If the method name is empty, it indicates a super constructor call. This structure supports calling methods from parent classes during interpretation.