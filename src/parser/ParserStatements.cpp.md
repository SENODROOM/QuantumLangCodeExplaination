# Quantum Language Compiler - ParserStatements.cpp

## Role in Compiler Pipeline

`ParserStatements.cpp` is a crucial component of the Quantum Language compiler's parsing phase. It focuses on converting individual statements from the source code into Abstract Syntax Tree (AST) nodes. This stage is essential as it forms the backbone for subsequent compilation phases, enabling more sophisticated analysis and transformation of the code.

## Key Design Decisions and WHY

1. **Decorator Handling**: The parser includes logic to skip over Python-style decorators such as `@property` or `@dataclass`. This decision was made to support a broader range of programming styles and syntaxes within the same compiler framework, enhancing its versatility.

2. **Storage Class Specifiers**: The parser also skips over C/C++ storage class specifiers like `static`, `extern`, `inline`, etc. This allows the compiler to handle these modifiers without treating them as separate entities, simplifying the AST structure.

3. **Type Hint Parsing**: For C-style variable declarations with type qualifiers (like `const int*`), the parser treats the entire type hint as a single entity. This approach ensures that type information is accurately captured and represented in the AST, facilitating easier type checking and optimization during later stages of compilation.

## Major Classes/Functions Overview

### `Parser::parseStatement()`
- **Functionality**: This function is the entry point for parsing individual statements. It handles various types of statements including variable declarations (`let`, `const`).
- **Process**:
  1. Skips any leading newlines.
  2. Optionally consumes decorators.
  3. Skips any preceding storage class specifiers.
  4. Depending on the statement type (`let`, `const`), calls specific functions to parse the declaration.
  5. Handles multiple declarations separated by commas.

### Helper Functions
- **`skipNewlines()`**: Skips over any newline tokens.
- **`consume()`**: Consumes the current token and advances the parser.
- **`check(TokenType type)`**: Checks if the current token matches the specified token type.
- **`isCTypeKeyword(TokenType type)`**: Determines if the given token type represents a C/C++ type keyword.

## Tradeoffs

1. **Flexibility vs Complexity**: By supporting both Python and C-like syntaxes, the parser becomes more complex but offers greater flexibility in handling different programming paradigms.

2. **Performance vs Accuracy**: The decision to treat type qualifiers as part of the type hint simplifies the AST but might reduce performance slightly due to additional processing required for type extraction.

3. **Maintainability vs Feature Set**: Supporting multiple languages and syntax styles increases the feature set but can complicate maintenance and testing efforts.

Overall, `ParserStatements.cpp` plays a vital role in the Quantum Language compiler by providing robust mechanisms for parsing and representing statements in the AST. These features enable the compiler to handle diverse programming styles efficiently, making it a versatile tool for quantum programming environments.