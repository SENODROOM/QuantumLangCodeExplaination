# Quantum Language Compiler - ParserStatements.cpp

## Role in Compiler Pipeline

`ParserStatements.cpp` is a critical part of the Quantum Language compiler's parsing phase. Its primary function is to convert individual statements from the source code into Abstract Syntax Tree (AST) nodes. This step is fundamental because it lays the groundwork for semantic analysis and code generation. By accurately parsing each statement, the compiler can build a structured representation of the program that facilitates further processing stages.

## Key Design Decisions and Why

### Handling Decorators
One of the unique features implemented in `ParserStatements.cpp` is the ability to handle Python-style decorators. These are often used in various programming languages to modify or enhance functions or methods. The parser skips these decorators to ensure they do not interfere with the main statement parsing process. This decision was made to maintain compatibility across different programming paradigms and to allow users to utilize decorators without disrupting the core syntax of the language.

### Storage Class Specifiers
The parser also includes functionality to skip over C/C++ storage class specifiers such as `static`, `extern`, `inline`, `volatile`, `register`, and `mutable`. These specifiers provide additional information about variable scope, linkage, and optimization but do not affect the syntactic structure of the code. By ignoring these specifiers, the parser ensures that only relevant information is captured in the AST, simplifying subsequent phases of the compilation process.

### Type Keywords
To support both Quantum and classical types, the parser uses a combination of custom type keywords and existing C-type keywords. For example, it recognizes `let` for quantum variables and handles classical type keywords like `int`, `float`, etc. This dual approach allows the compiler to seamlessly integrate quantum programming concepts with traditional C-like syntax, making it versatile and user-friendly.

## Major Classes/Functions Overview

### Parser Class
- **Role**: Manages the overall parsing process, including tokenization, lookahead, and error handling.
- **Key Functions**:
  - `parseStatement()`: Parses individual statements and returns their corresponding AST nodes.
  - `skipNewlines()`: Skips any newline characters to ensure consistent line numbering.
  - `consume()`: Consumes the current token and advances to the next one.
  - `check(TokenType type)`: Checks if the current token matches the specified type without advancing.

### ASTNode Class
- **Role**: Represents a node in the Abstract Syntax Tree (AST), which is a hierarchical structure that captures the syntactic elements of the source code.
- **Key Functions**:
  - `as<T>()`: Template method to cast the ASTNode to a specific type (e.g., BlockStmt, VarDecl).
  - `push_back(ASTNodePtr node)`: Adds a child node to the current ASTNode.

## Tradeoffs

### Complexity vs. Flexibility
By supporting both quantum and classical types through a hybrid approach, `ParserStatements.cpp` increases complexity. However, this flexibility allows the compiler to cater to a broader audience and enables seamless integration between quantum and classical programming paradigms.

### Performance vs. Usability
Skipping decorators and storage class specifiers improves performance by reducing unnecessary checks during the parsing process. On the other hand, this might reduce usability for developers who rely heavily on these features for certain applications.

### Maintainability vs. Extensibility
The use of custom type keywords and template methods enhances maintainability by clearly distinguishing between quantum and classical types. However, this could make the codebase more difficult to extend if new features require additional syntax changes.

Overall, `ParserStatements.cpp` is a well-designed component of the Quantum Language compiler that balances complexity, performance, and usability to create a robust and flexible parsing system.