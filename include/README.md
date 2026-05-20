# QuantumLanguage Compiler - AST.h

## Overview

The `include/AST.h` header file is an essential part of the QuantumLanguage compiler, responsible for defining the Abstract Syntax Tree (AST). The AST represents the syntactic structure of the source code in a tree-like format, making it easier to analyze and process by the compiler. This file includes various structures representing different types of expressions and statements found in the source code.

## Role in Compiler Pipeline

The AST serves as the intermediate representation between the source code and the executable code. It is generated during the parsing phase of the compilation process. The AST is then used by subsequent phases such as semantic analysis, optimization, and code generation to transform the source code into its final executable form.

## Key Design Decisions and Why

### Use of Variants

The AST uses `std::variant` to represent different types of nodes. This choice allows for a flexible and type-safe way to handle various expression and statement types within a single data structure. By using `std::variant`, we avoid the need for multiple inheritance and can easily add new node types without breaking existing code.

### Unique Pointers

To manage memory efficiently, the AST uses `std::unique_ptr` for all node pointers. This ensures that each node is properly owned and deleted when it is no longer needed, preventing memory leaks and dangling pointers.

### Separate Expression and Statement Types

The AST distinguishes between expression and statement types, which helps in better understanding and processing of the code. Expressions evaluate to a value, while statements perform actions without returning a value.

## Major Classes/Functions Overview

### Expression Types

- **NumberLiteral**: Represents a numeric literal.
- **StringLiteral**: Represents a string literal.
- **BoolLiteral**: Represents a boolean literal.
- **NilLiteral**: Represents the `nil` literal.
- **Identifier**: Represents an identifier (variable name).
- **BinaryExpr**: Represents a binary operation (e.g., `+`, `-`, `*`, `/`).
- **UnaryExpr**: Represents a unary operation (e.g., `!`, `-`).
- **AssignExpr**: Represents an assignment operation (e.g., `=`, `+=`, `-=`, `*=`, `/=`).
- **CallExpr**: Represents a function call.
- **IndexExpr**: Represents indexing into an array or dictionary.
- **SliceExpr**: Represents slicing operations similar to Python's `[start:stop:step]`.
- **MemberExpr**: Represents accessing a member of an object.
- **ArrayLiteral**: Represents an array literal.
- **DictLiteral**: Represents a dictionary literal.
- **LambdaExpr**: Represents a lambda function with optional parameter types and return type hints.
- **TernaryExpr**: Represents a ternary conditional expression.
- **SuperExpr**: Represents calling a superclass constructor or method.

### C++ Pointer Expression Types

- **AddressOfExpr**: Represents the address-of operator (`&`).
- **DerefExpr**: Represents the dereference operator (`*`).
- **ArrowExpr**: Represents the arrow operator (`->`) for accessing members through pointers.

### Statement Types

- **VarDecl**: Represents a variable declaration.
- **FunctionDecl**: Represents a function declaration with optional parameter types and return type hints.
- **ReturnStmt**: Represents a return statement.
- **IfStmt**: Represents an if statement with optional `elif` chains.

## Tradeoffs

Using `std::variant` for node types simplifies the implementation but increases the complexity of type checking and casting. Using `std::unique_ptr` for memory management ensures proper ownership and deletion of nodes but requires additional overhead for smart pointer usage. Separating expression and statement types improves clarity but adds some redundancy in handling certain cases.

Overall, these design choices aim to provide a balance between simplicity, safety, and flexibility in the AST representation.