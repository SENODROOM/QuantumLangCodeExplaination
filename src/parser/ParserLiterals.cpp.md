# ParserLiterals.cpp

## Role in Compiler Pipeline

`ParserLiterals.cpp` plays a crucial role in the Quantum Language compiler's parsing stage. It is responsible for interpreting and converting literal values like numbers, strings, booleans, `nil`, identifiers (`this`), and memory allocations (`new`) into their corresponding Abstract Syntax Tree (AST) nodes. This transformation facilitates further processing during the compilation phase.

## Key Design Decisions and Why

1. **Handling Different Literal Types**: The parser must distinguish between different literal types such as integers, floating-point numbers, strings, booleans, and `nil`. Each type requires specific handling to ensure accurate conversion into AST nodes.

2. **Memory Allocation Parsing**: The parser needs to correctly interpret memory allocation expressions (`new`). This includes handling both heap allocation and array allocation, ensuring that the resulting AST accurately reflects the intended memory structure.

3. **Identifier Handling**: The parser must recognize and convert the `this` keyword into an appropriate AST node representing the current object context. This is essential for maintaining proper scope and object-oriented semantics in the compiled code.

4. **Error Handling**: Robust error handling is critical to provide clear feedback when unexpected tokens or syntax errors occur. The parser throws exceptions with descriptive messages to aid debugging and improve user experience.

## Major Classes/Functions Overview

### Parser Class
- **Functionality**: Manages the overall parsing process, including tokenization, lookahead, and error reporting.
- **Key Methods**:
  - `parsePrimary()`: Parses primary literals (numbers, strings, booleans, `nil`, `this`, and `new`).
  - `parseExpr()`: Parses expressions, which can include literals, variables, and operations.
  - `parseArgList()`: Parses argument lists for functions and constructors.

### ASTNode Class
- **Purpose**: Represents a node in the Abstract Syntax Tree (AST). Each node corresponds to a part of the source code, making it easier to manipulate and analyze the code structure.
- **Subclasses**:
  - `NumberLiteral`: Represents numeric literals.
  - `StringLiteral`: Represents string literals.
  - `BoolLiteral`: Represents boolean literals.
  - `NilLiteral`: Represents the `nil` literal.
  - `Identifier`: Represents variable or object identifiers.
  - `NewExpr`: Represents memory allocation expressions.

## Tradeoffs

1. **Flexibility vs. Complexity**: Allowing both C++ primitive type keywords and identifier class names for memory allocation simplifies usage but increases complexity in the parser implementation. Ensuring correct parsing and error handling for these variations adds overhead.

2. **Performance vs. Error Tolerance**: Implementing thorough error checking improves code quality but may reduce performance due to additional exception handling. Balancing these factors ensures robustness without compromising efficiency.

3. **Readability vs. Functionality**: Adding support for advanced features like memory allocation arrays enhances functionality but may decrease readability for beginners. Prioritizing simplicity over comprehensive feature set helps maintain clarity and ease of use.

By addressing these tradeoffs, `ParserLiterals.cpp` provides a well-rounded solution for parsing literals and memory allocations, contributing significantly to the overall reliability and performance of the Quantum Language compiler.