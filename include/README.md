# QuantumLanguage Compiler - AST.h

## Overview

The `include/AST.h` header file is essential to the QuantumLanguage compiler by defining the Abstract Syntax Tree (AST). The AST represents the syntactic structure of a program in a tree-like format, making it easier for the compiler to analyze, transform, and optimize the code. This file includes various structures representing different types of expressions and statements used in the language.

## Role in Compiler Pipeline

The AST serves as the intermediate representation between the source code and the machine code. During compilation, the source code is parsed into an AST, which is then analyzed, transformed, and optimized before being translated into executable code. The AST facilitates error detection, semantic analysis, and code generation phases.

## Key Design Decisions and Why

1. **Expression Variants**: The use of `std::variant` allows for a flexible and efficient representation of various expression types within the AST. This approach avoids multiple inheritance and reduces memory overhead compared to traditional union-based designs.

2. **Memory Management**: By using smart pointers (`std::unique_ptr`), the AST ensures proper memory management. Each node in the AST has exclusive ownership of its children, preventing dangling references and ensuring that resources are deallocated correctly when nodes are destroyed.

3. **Type Safety**: The introduction of explicit type hints for variables and function parameters enhances type safety. This feature helps catch type-related errors early in the compilation process, reducing runtime issues.

4. **Extensibility**: The AST design is modular and extensible. New expression and statement types can be easily added without modifying existing code, allowing the language to evolve over time.

## Major Classes/Functions Overview

### Expression Types

- **NumberLiteral**: Represents numeric literals.
- **StringLiteral**: Represents string literals.
- **BoolLiteral**: Represents boolean literals.
- **NilLiteral**: Represents the nil literal.
- **Identifier**: Represents variable identifiers.
- **BinaryExpr**: Represents binary operations like addition, subtraction, etc.
- **UnaryExpr**: Represents unary operations like negation, increment, etc.
- **AssignExpr**: Represents assignment operations with support for compound assignments.
- **CallExpr**: Represents function calls.
- **IndexExpr**: Represents array indexing.
- **SliceExpr**: Represents slicing operations similar to Python's syntax.
- **MemberExpr**: Represents member access on objects.
- **ArrayLiteral**: Represents array literals.
- **DictLiteral**: Represents dictionary literals.
- **LambdaExpr**: Represents anonymous functions with optional parameter types and return types.
- **TernaryExpr**: Represents conditional expressions (if-then-else).
- **SuperExpr**: Represents calls to superclass constructors or methods.

### C++ Pointer Expression Types

- **AddressOfExpr**: Represents the address-of operator (`&`).
- **DerefExpr**: Represents the dereference operator (`*`).
- **ArrowExpr**: Represents member access through pointers (`->`).

### Statement Types

- **VarDecl**: Represents variable declarations, including constness, type hints, and pointer status.
- **FunctionDecl**: Represents function declarations, including parameter types, default arguments, and return types.
- **ReturnStmt**: Represents return statements, optionally containing a return value.
- **IfStmt**: Represents if statements, with conditions and branches.

## Tradeoffs

- **Flexibility vs. Complexity**: While `std::variant` provides flexibility, it also adds complexity to the AST implementation. Careful consideration must be given to ensure that the variant usage does not lead to performance bottlenecks or increased memory consumption.
  
- **Memory Overhead**: Using smart pointers incurs some memory overhead due to reference counting and additional control blocks. However, this is generally outweighed by the benefits of automatic memory management and reduced risk of memory leaks.

- **Type Safety vs. Usability**: Explicit type hints enhance type safety but may make the AST more verbose and harder to read for simple cases. Balancing these factors requires careful consideration of how often type hints are necessary versus their impact on readability.

- **Extensibility vs. Maintenance**: Adding new expression and statement types increases the maintainability of the AST by providing a clear and organized structure. However, it also means that changes to the AST may require updates throughout the compiler, potentially increasing maintenance costs.

Overall, the `include/AST.h` header file plays a critical role in the QuantumLanguage compiler by providing a robust and flexible representation of the language's syntax. Its design choices balance various considerations to ensure efficient and reliable code generation.