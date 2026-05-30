# Quantum Language Compiler - ParserStatements.cpp

## Role in Compiler Pipeline

`ParserStatements.cpp` is an integral part of the Quantum Language compiler's parsing phase. Its primary function is to convert individual statements from the source code into Abstract Syntax Tree (AST) nodes. This stage is pivotal because it lays the foundation for the subsequent compilation phases, facilitating more intricate analyses and transformations.

## Key Design Decisions and Why

### Handling Decorators

The parser includes functionality to handle Python-style decorators such as `@property`, `@dataclass`. This decision was made to accommodate a broader range of programming paradigms within the Quantum Language, ensuring compatibility with existing Pythonic patterns. By skipping these decorators during parsing, the compiler can focus on the core logic of quantum programs without being distracted by syntactic sugar.

### Storage Class Specifiers

To support C/C++-style storage class specifiers like `static`, `extern`, `inline`, etc., the parser has been designed to recognize and skip them. This approach allows the compiler to maintain a consistent internal representation of variables, regardless of their storage class in the source code. The tradeoff here is that some potential optimizations related to storage class may be lost or require additional handling later in the compilation process.

## Major Classes/Functions Overview

### Parser Class

The `Parser` class contains the main logic for parsing statements. It includes methods like `parseStatement()` which orchestrates the parsing process, and helper functions to consume tokens and check token types.

### ASTNode Class

This class represents a node in the Abstract Syntax Tree. Each node corresponds to a statement or expression in the source code. The `ParserStatements.cpp` file uses various subclasses of `ASTNode` to construct different parts of the AST.

### parseStatement Function

The `parseStatement()` function is central to the module. It handles the parsing of individual statements, starting with skipping any leading newlines and decorators. It then checks for storage class specifiers and proceeds to parse variable declarations (`let` or `const`) based on the encountered token type.

## Tradeoffs

### Flexibility vs. Complexity

By supporting both Python-like and C-like syntax for variable declarations, the parser increases flexibility but adds complexity to the implementation. This requires careful handling of different syntax patterns and ensures that the compiler can correctly interpret and translate them into its internal AST format.

### Internal Representation Consistency

Skipping storage class specifiers simplifies the internal representation of variables within the AST. However, this might lead to loss of information about how variables should be stored or accessed at runtime, necessitating additional steps during the optimization or code generation phases.

Overall, `ParserStatements.cpp` serves as a robust component of the Quantum Language compiler, adeptly handling diverse syntax patterns while maintaining a clean and efficient internal representation of the parsed code.