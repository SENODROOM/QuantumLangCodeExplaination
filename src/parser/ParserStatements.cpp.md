# Quantum Language Compiler - ParserStatements.cpp

## Role in Compiler Pipeline

`ParserStatements.cpp` is an essential component of the Quantum Language compiler's parsing phase. It focuses on parsing individual statements within the source code into Abstract Syntax Tree (AST) nodes. This file plays a crucial role in translating the syntactic structure of the code into a hierarchical representation that can be easily analyzed and manipulated by subsequent phases of the compiler, such as semantic analysis and code generation.

## Key Design Decisions and Why

### Handling Decorators
The parser includes specific handling for Python-style decorators, which are not part of the standard C++ syntax. This decision was made to support a broader range of programming paradigms and styles within the Quantum Language ecosystem. By skipping these decorators, the parser ensures that the core functionality of the language remains intact without interference from external annotations.

### Storage Class Specifiers
The parser also skips over C/C++ storage class specifiers like `static`, `extern`, `inline`, etc. This choice was driven by the need to maintain compatibility with existing C/C++ codebases while allowing the Quantum Language to introduce its own unique features. By ignoring these specifiers, the parser simplifies the AST construction process, focusing instead on the actual declarations and expressions.

### Type Hint Parsing
A significant feature of `ParserStatements.cpp` is its ability to handle type hints, both in C-style (`const int`) and more modern C++-style (`int const`). This dual approach allows the parser to flexibly accommodate different coding conventions and preferences. The use of type hints enhances the readability and maintainability of the generated AST, making it easier for developers to understand the intended data types of variables and expressions.

## Major Classes/Functions Overview

### Parser Class
The primary class in `ParserStatements.cpp` is the `Parser` class, which contains methods for parsing various elements of the source code. One of its key methods is `parseStatement()`, which is responsible for parsing individual statements.

#### Functions Within Parser Class
- **skipNewlines()**: Skips any newline characters or comments in the input stream.
- **check(TokenType type)**: Checks if the current token matches the specified token type.
- **consume()**: Consumes the current token and advances to the next one.
- **parseVarDecl(bool mutableAllowed)**: Parses variable declarations, including both mutable and immutable cases.
- **parseCTypeVarDecl(const std::string& typeHint)**: Parses variable declarations using C-style type hints.
- **isCTypeKeyword(TokenType type)**: Determines if the given token type represents a valid C/C++ type keyword.

### ASTNode Class
The `ASTNode` class serves as the base class for all nodes in the Abstract Syntax Tree. It encapsulates common properties such as the node type and line number. Specific subclasses, like `BlockStmt`, represent more complex structures within the AST.

#### BlockStmt Subclass
The `BlockStmt` subclass extends `ASTNode` and represents a block of statements. It contains a vector of pointers to `ASTNode` objects, each representing a statement within the block.

## Tradeoffs

### Flexibility vs. Complexity
By supporting both Python-style decorators and C-style type hints, the parser increases flexibility but adds complexity to the implementation. Developers must account for these additional syntax elements during AST traversal and manipulation.

### Compatibility vs. Innovation
Allowing storage class specifiers like `static` and `extern` maintains compatibility with existing C/C++ codebases. However, this might limit the introduction of new, innovative features in the Quantum Language that could otherwise simplify the codebase.

### Readability vs. Performance
Using type hints improves the readability of the generated AST, making it easier for developers to understand the code structure. However, this could potentially impact performance, especially when parsing large codebases with numerous type hints.

Overall, `ParserStatements.cpp` is a critical piece of the Quantum Language compiler, balancing multiple factors to ensure robustness, flexibility, and ease of development. Its design choices reflect a commitment to supporting a wide range of programming styles while maintaining a clean and efficient AST structure.