# Quantum Language Compiler - ParserStatements.cpp

## Role in Compiler Pipeline

`ParserStatements.cpp` plays a crucial role in the Quantum Language compiler's parsing phase. It converts individual statements from the source code into Abstract Syntax Tree (AST) nodes. This stage is essential because it forms the backbone for subsequent phases such as semantic analysis and code generation.

### Key Design Decisions and Why

1. **Decorator Handling**: The parser includes specific logic to handle Python-style decorators (`@property`, `@dataclass`). These are skipped during the parsing process since they do not affect the quantum language syntax. This decision ensures that the parser remains focused on the core quantum language constructs without unnecessary complexity.

2. **Storage Class Specifiers**: The parser also skips C/C++ storage class specifiers such as `static`, `extern`, `inline`, etc. These specifiers are ignored as they do not influence the quantum language semantics. By skipping these, the parser simplifies its task and maintains focus on the relevant quantum language elements.

3. **Type Hint Parsing**: The parser uses a mechanism to handle type hints, particularly for C-style declarations where `const` can precede or follow the type keyword. This allows for flexible parsing of variable declarations and ensures compatibility with existing C/C++ codebases when used within the quantum language context.

## Major Classes/Functions Overview

### `Parser::parseStatement()`
- **Purpose**: Converts individual statements from the source code into AST nodes.
- **Functionality**:
  - Skips new lines using `skipNewlines()`.
  - Handles Python-style decorators by consuming them until a non-decorator token is encountered.
  - Ignores C/C++ storage class specifiers.
  - Parses different types of statements such as `let` and `const` declarations based on the current token type.
  - Utilizes helper functions like `consume()` and `check()` to manage token consumption and validation.

### Helper Functions
- **`Parser::skipNewlines()`**: Consumes newline tokens to ensure the parser moves past any blank lines.
- **`Parser::consume()`**: Advances the parser to the next token and returns it.
- **`Parser::check(TokenType type)`**: Checks if the current token matches the specified type without advancing the parser.
- **`Parser::isCTypeKeyword(TokenType type)`**: Determines if the current token represents a C/C++ type keyword.

## Trade-offs

1. **Flexibility vs Simplicity**: Allowing the parser to handle both quantum-specific and C/C++ constructs introduces flexibility but increases complexity. By ignoring certain C/C++ features, the parser simplifies its implementation and reduces potential errors related to unsupported syntax.

2. **Performance vs Completeness**: Ignoring storage class specifiers and decorators might slightly reduce performance due to additional checks and skips. However, this trade-off is justified by the need for a more maintainable and focused parser.

3. **Compatibility vs Isolation**: Supporting C-style type declarations enhances compatibility with existing codebases, which could be beneficial for adoption. On the other hand, isolating the quantum language syntax might make the parser easier to understand and modify over time.

In conclusion, `ParserStatements.cpp` is a vital component of the Quantum Language compiler, responsible for converting source code statements into AST nodes. Through strategic design decisions, it balances flexibility, simplicity, performance, and compatibility to effectively support the quantum language syntax while maintaining robustness and scalability.