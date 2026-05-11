# QuantumLanguage Compiler - AST.h

## Overview

The `include/AST.h` header file is an essential part of the QuantumLanguage compiler, dedicated to defining the Abstract Syntax Tree (AST). The AST represents the syntactic structure of the source code in a tree-like format, where each node corresponds to a construct in the language. This representation facilitates parsing, semantic analysis, and code generation phases of the compilation process.

## Role in Compiler Pipeline

The AST serves as the intermediate representation between the source code and the final executable code. It is constructed during the parsing phase and used throughout the rest of the compiler pipeline:

- **Semantic Analysis**: The AST is analyzed to ensure that the code adheres to the language's rules and constraints.
- **Code Generation**: The AST is transformed into machine code or another lower-level representation suitable for execution.
- **Optimization**: The AST can be optimized to improve performance before code generation.

### Key Design Decisions and Why

1. **Use of Variants**: The AST nodes are defined using `std::variant`, allowing for a flexible and extensible design. Each variant alternative represents a different type of AST node, enabling the compiler to handle various constructs uniformly.
2. **Smart Pointers**: The use of `std::unique_ptr` ensures that memory management is handled correctly, preventing memory leaks and dangling pointers.
3. **Forward Declarations**: Forward declarations of struct types like `ASTNode` help reduce compile-time dependencies and improve build times.

## Major Classes/Functions Overview

### Expression Types

- **NumberLiteral**: Represents a numeric literal in the source code.
- **StringLiteral**: Represents a string literal in the source code.
- **BoolLiteral** and **NilLiteral**: Represent boolean and nil literals respectively.
- **Identifier**: Represents an identifier (variable or function name).
- **BinaryExpr**: Represents binary expressions with operators and operands.
- **UnaryExpr**: Represents unary expressions with an operator and an operand.
- **AssignExpr**: Represents assignment expressions with an operator, target variable, and value.
- **CallExpr**: Represents function calls with a callee and arguments.
- **IndexExpr**: Represents indexing operations with an object and an index.
- **SliceExpr**: Represents slicing operations similar to Python's syntax.
- **MemberExpr**: Represents member access operations with an object and a member name.
- **ArrayLiteral**: Represents array literals with elements.
- **DictLiteral**: Represents dictionary literals with key-value pairs.
- **LambdaExpr**: Represents lambda functions with parameters, parameter types, default arguments, return type, and body.
- **TernaryExpr**: Represents ternary conditional expressions with a condition, then expression, and else expression.
- **SuperExpr**: Represents super constructor or method calls.

### C++ Pointer Expression Types

- **AddressOfExpr**: Represents the address-of operator (`&`).
- **DerefExpr**: Represents the dereference operator (`*`).
- **ArrowExpr**: Represents the arrow operator (`->`) for accessing members through pointers.

### Statement Types

- **VarDecl**: Represents variable declarations, including whether they are constants, their names, initializers, type hints, and whether they are pointers.
- **FunctionDecl**: Represents function declarations, including their names, parameters, parameter types, default arguments, return type, and bodies.
- **ReturnStmt**: Represents return statements, which may contain a return value.
- **IfStmt**: Represents if statements, which include conditions and then branches.

## Tradeoffs

- **Flexibility vs. Complexity**: Using `std::variant` provides flexibility but increases complexity in terms of handling different node types.
- **Memory Management**: Smart pointers (`std::unique_ptr`) simplify memory management but add overhead compared to raw pointers.
- **Readability vs. Performance**: The use of forward declarations improves readability but might slightly impact performance due to additional indirections.

Overall, the `AST.h` header file plays a critical role in the QuantumLanguage compiler by providing a structured and extensible representation of the source code. Its design choices balance flexibility, readability, and performance, making it well-suited for the complexities of modern programming languages.