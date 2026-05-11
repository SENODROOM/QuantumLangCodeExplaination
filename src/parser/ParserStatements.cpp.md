# Quantum Language Compiler - ParserStatements.cpp

## Role in Compiler Pipeline

`ParserStatements.cpp` plays a crucial role in the Quantum Language compiler's parsing phase. It converts individual statements from the source code into Abstract Syntax Tree (AST) nodes. This step is essential because it sets the foundation for subsequent stages such as semantic analysis and code generation. By accurately parsing statements, `ParserStatements.cpp` ensures that the compiler can understand and process the quantum code correctly.

## Key Design Decisions and Why

### Handling Decorators
The parser includes functionality to skip Python-style decorators (`@property`, `@dataclass`). This is done to maintain compatibility with various programming paradigms and to ensure that the core parsing logic remains focused on quantum-specific syntax.

### Storage Class Specifiers
To handle C/C++ storage class specifiers like `static`, `extern`, `inline`, `volatile`, `register`, and `mutable`, the parser checks for these identifiers and skips them. This decision allows the compiler to support mixed-language projects or legacy codebases without interfering with the quantum language's syntax.

### Type Keywords
The parser distinguishes between quantum-specific type keywords and C-style type qualifiers. For example, it treats `"const int"` as a C-type variable declaration rather than a quantum constant. This approach ensures that quantum code is parsed according to its specific rules, while still allowing for interop with existing C/C++ code.

## Major Classes/Functions Overview

### Parser Class
- **Functionality**: The main parser class responsible for converting tokens into AST nodes.
- **Key Functions**:
  - `parseStatement()`: Parses an individual statement and returns an AST node.
  - `skipNewlines()`: Skips any newline characters in the input stream.
  - `consume()`: Consumes the current token and advances to the next one.
  - `check(TokenType type)`: Checks if the current token matches the specified type.

### ASTNode Class
- **Role**: Represents a node in the Abstract Syntax Tree.
- **Subclasses**:
  - `BlockStmt`: Represents a block of statements.
  - `VarDecl`: Represents a variable declaration.
  - `ConstDecl`: Represents a constant declaration.

## Tradeoffs

### Complexity vs. Flexibility
By handling both quantum-specific and C-style syntax within the same parser, the implementation becomes more complex but offers greater flexibility. This allows the compiler to support mixed-language projects or legacy codebases without significant changes to the core parsing logic.

### Performance vs. Accuracy
The parser optimizes performance by skipping unnecessary tokens (like decorators and storage class specifiers). However, this may compromise accuracy in certain edge cases where these tokens could be valid in other contexts. To mitigate this, additional validation steps can be introduced during semantic analysis.

### Interoperability vs. Isolation
Allowing the parser to handle C-style type qualifiers provides interoperability with existing C/C++ codebases. However, this may introduce potential conflicts or ambiguities when parsing mixed-language code. To address this, careful design and testing are required to ensure consistent behavior across different languages.

In conclusion, `ParserStatements.cpp` is a vital component of the Quantum Language compiler's parsing phase, providing accurate and flexible conversion of statements into AST nodes. While it introduces some complexity and potential tradeoffs, the benefits of supporting mixed-language projects and maintaining compatibility with existing codebases make it a worthwhile investment.