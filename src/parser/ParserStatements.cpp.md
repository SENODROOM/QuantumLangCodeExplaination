# Quantum Language Compiler - ParserStatements.cpp

## Role in Compiler Pipeline

`ParserStatements.cpp` plays a crucial role in the Quantum Language compiler's parsing phase by converting individual statements from the source code into Abstract Syntax Tree (AST) nodes. This stage is essential as it forms the backbone for subsequent compilation stages, enabling more sophisticated analysis and transformation of the code.

## Key Design Decisions and Why

### Handling Decorators
The parser includes specific handling for Python-style decorators such as `@property`, `@dataclass`. These are skipped during the initial parsing process to ensure that they do not interfere with the main statement parsing logic. This decision allows the compiler to maintain compatibility with both Python and Quantum languages without additional complexity.

### Storage Class Specifiers
To support C/C++ storage class specifiers like `static`, `extern`, `inline`, etc., the parser checks for these identifiers and skips them before proceeding with statement parsing. This ensures that the core language features are correctly identified and processed.

### Type Hint Parsing
The implementation includes specialized functions to handle type hints, particularly those used in C/C++. By reusing existing logic for parsing type keywords, the parser can efficiently manage different types of declarations, including those with qualifiers like `const`.

## Major Classes/Functions Overview

### Parser Class
- **Functionality**: The `Parser` class contains the main logic for parsing statements. It manages the token stream and constructs AST nodes based on the parsed syntax.
- **Key Methods**:
  - `parseStatement()`: Parses a single statement from the input.
  - `skipNewlines()`: Skips over any newline characters in the input.
  - `consume()`: Consumes the current token and advances to the next one.
  - `check(TokenType type)`: Checks if the current token matches the specified type without advancing.

### ASTNode Class
- **Functionality**: Represents a node in the Abstract Syntax Tree. Different subclasses (`BlockStmt`, `VarDecl`, etc.) represent various types of statements and declarations.
- **Key Methods**:
  - `as<T>()`: Casts the ASTNode to a specific subclass type `T`.
  - `push_back(ASTNodePtr stmt)`: Adds a new statement to a block.

## Tradeoffs

### Compatibility vs. Simplicity
By skipping Python-style decorators and C/C++ storage class specifiers, the parser maintains compatibility with multiple languages but adds some complexity to the main parsing logic. This tradeoff balances broad language support against simplicity in the core parser.

### Efficiency vs. Flexibility
Reusing existing logic for parsing type keywords improves efficiency but limits flexibility in handling unique type declarations or qualifiers. This tradeoff prioritizes performance over comprehensive customization options.

In summary, `ParserStatements.cpp` is a vital part of the Quantum Language compiler's parsing infrastructure, designed to handle a variety of language-specific features while maintaining efficiency and broad compatibility.