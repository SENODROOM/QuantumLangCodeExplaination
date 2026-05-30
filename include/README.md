# QuantumLanguage Compiler - AST.h

## Overview

The `include/AST.h` header file is a crucial component of the QuantumLanguage compiler, focusing on defining the Abstract Syntax Tree (AST). The AST represents the syntactic structure of a source code in a tree-like format, making it easier for the compiler to understand and process the code.

## Role in Compiler Pipeline

The AST serves as the intermediate representation between the source code and the executable code. It is constructed during the parsing phase of the compilation process. The AST is then used by various stages of the compiler, including semantic analysis, optimization, and code generation. By using the AST, the compiler can perform operations such as type checking, scope resolution, and dead code elimination more efficiently.

## Key Design Decisions and Why

### Node Variants

The AST is designed using a variant to represent different types of nodes. Each node type corresponds to a specific construct in the language, such as literals, expressions, statements, and function declarations. This approach allows for a flexible and extensible representation of the language syntax.

### Smart Pointers

To manage memory effectively and avoid memory leaks, smart pointers (`std::unique_ptr`) are used to store pointers to AST nodes. This ensures that each node is properly deallocated when it is no longer needed.

### Type Information

Type information is included in many AST node types, allowing the compiler to maintain precise knowledge about the data types involved. For example, literal nodes like `NumberLiteral`, `StringLiteral`, and `BoolLiteral` store their respective values along with their type hints.

## Major Classes/Functions Overview

### ASTNode

This is the base class for all AST nodes. It contains common properties such as location information and a parent pointer.

### Literal Nodes

- **NumberLiteral**: Represents numeric literals.
- **StringLiteral**: Represents string literals.
- **BoolLiteral**: Represents boolean literals.
- **NilLiteral**: Represents the nil value.

### Identifier

Represents variable names and other identifiers.

### Expressions

- **BinaryExpr**: Represents binary operators.
- **UnaryExpr**: Represents unary operators.
- **AssignExpr**: Represents assignment expressions.
- **CallExpr**: Represents function calls.
- **IndexExpr**: Represents indexing expressions.
- **SliceExpr**: Represents slicing expressions.
- **MemberExpr**: Represents member access expressions.
- **ArrayLiteral**: Represents array literals.
- **DictLiteral**: Represents dictionary literals.
- **LambdaExpr**: Represents lambda functions.
- **TernaryExpr**: Represents ternary conditional expressions.
- **SuperExpr**: Represents calls to the superclass constructor or methods.

### C++ Pointer Expressions

- **AddressOfExpr**: Represents the address-of operator (`&`).
- **DerefExpr**: Represents the dereference operator (`*`).
- **ArrowExpr**: Represents the arrow operator (`->`).

### Statements

- **VarDecl**: Represents variable declarations.
- **FunctionDecl**: Represents function declarations.
- **ReturnStmt**: Represents return statements.
- **IfStmt**: Represents if statements.

## Tradeoffs

### Memory Management

Using smart pointers (`std::unique_ptr`) for managing AST nodes helps prevent memory leaks but adds some overhead compared to raw pointers.

### Flexibility vs. Complexity

The use of variants to represent different node types provides flexibility in handling various constructs but increases the complexity of the codebase.

### Performance

While the AST provides a clear and structured representation of the code, its traversal and manipulation can impact performance, especially in larger programs. Optimizations and caching strategies are necessary to mitigate these effects.

By understanding the role of the AST in the QuantumLanguage compiler's pipeline and the key design decisions behind its implementation, developers can better appreciate how the compiler processes and generates code.