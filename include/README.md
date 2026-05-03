# QuantumLanguage Compiler - AST.h

## Overview

The `include/AST.h` header file is an essential part of the QuantumLanguage compiler, focusing on defining the Abstract Syntax Tree (AST). The AST serves as a structured representation of the source code, enabling efficient parsing, analysis, and transformation during the compilation process.

## Role in Compiler Pipeline

The AST plays a pivotal role in the QuantumLanguage compiler's pipeline:
1. **Parsing**: Converts the input source code into an AST.
2. **Analysis**: Validates the syntax and semantics of the code.
3. **Transformation**: Modifies the AST to optimize the code or generate different representations.
4. **Code Generation**: Translates the AST into executable code.

By maintaining a clear and comprehensive AST, the compiler can effectively handle complex language features and ensure that the generated code adheres to the intended specifications.

## Key Design Decisions and Tradeoffs

### Node Variants

The AST nodes are defined using `std::variant`, allowing for a flexible and extensible structure. This choice simplifies the management of different node types without requiring multiple inheritance or additional type-checking mechanisms.

**Why**: `std::variant` provides a safer and more readable alternative to unions, making it easier to work with various node types within the same tree.

**Tradeoff**: While `std::variant` offers better safety and readability, it may introduce some performance overhead compared to traditional unions.

### Expression Types

The AST includes a variety of expression types such as literals, binary expressions, unary expressions, assignments, function calls, array and dictionary literals, lambda functions, ternary operators, and super expressions. Each expression type is represented by a struct containing relevant data members.

**Why**: A detailed set of expression types ensures that all possible constructs in the language are accurately represented, facilitating precise semantic analysis and code generation.

**Tradeoff**: Defining too many expression types can lead to increased complexity and maintenance overhead.

### C++ Pointer Expression Types

Additional structs are provided for handling C++ pointer expressions, including the address-of (`&var`), dereference (`*ptr`), and arrow (`ptr->member`) operators. These structs enhance the AST's ability to represent low-level C++ operations.

**Why**: Representing C++ pointer operations explicitly allows for more accurate translation of high-level quantum language constructs into lower-level C++ code.

**Tradeoff**: Adding these specific structs increases the AST's size and complexity, potentially impacting performance and memory usage.

### Statement Types

The AST also defines statement types such as variable declarations, function declarations, return statements, and conditional statements. Each statement type is encapsulated in its own struct, providing a clear separation of concerns.

**Why**: Separating statement types into distinct structs makes the AST easier to navigate and understand, improving maintainability and scalability.

**Tradeoff**: Creating separate structs for each statement type can lead to redundancy and increased boilerplate code.

## Major Classes/Functions Overview

- **ASTNode**: Base class for all AST nodes, utilizing `std::variant` to store different node types.
- **NumberLiteral**, **StringLiteral**, **BoolLiteral**, **NilLiteral**: Structs representing literal values.
- **Identifier**: Represents variable names or identifiers.
- **BinaryExpr**, **UnaryExpr**: Structures for binary and unary operations.
- **AssignExpr**: Represents assignment operations, including compound assignments.
- **CallExpr**: Models function calls, including method calls and constructor calls.
- **IndexExpr**: Handles indexing operations, similar to Python slicing.
- **SliceExpr**: Represents Python-style slicing operations.
- **MemberExpr**: Models member access, either through dot notation or pointers.
- **ArrayLiteral**, **DictLiteral**: Structures for array and dictionary literals.
- **LambdaExpr**: Represents anonymous functions, including parameter types and return types.
- **TernaryExpr**: Models conditional expressions.
- **SuperExpr**: Represents super constructor calls or method calls.
- **AddressOfExpr**, **DerefExpr**, **ArrowExpr**: Specific structs for handling C++ pointer operations.

These classes and functions form the backbone of the AST, ensuring that all aspects of the source code are accurately captured and manipulated throughout the compilation process.

## Conclusion

The `include/AST.h` header file is a crucial component of the QuantumLanguage compiler, providing a robust and flexible AST structure. By carefully designing the node variants and incorporating specific expression and statement types, the compiler can efficiently parse, analyze, transform, and generate code. While there are potential tradeoffs associated with these choices, the benefits in terms of accuracy and maintainability make them worthwhile investments for the compiler's success.