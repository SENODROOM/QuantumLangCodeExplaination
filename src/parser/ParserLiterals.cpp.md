# ParserLiterals.cpp

## Role in Compiler Pipeline

`ParserLiterals.cpp` is an essential component of the Quantum Language compiler's parsing phase. Its primary function is to interpret and convert various literal values such as numbers, strings, booleans, `nil`, identifiers (`this`), and memory allocations (`new`) into their corresponding Abstract Syntax Tree (AST) nodes. This conversion is critical because it enables subsequent stages of the compilation process to work with structured data representations rather than raw lexical tokens.

### Key Design Decisions and Why

1. **Literal Handling**: The parser must accurately distinguish between different types of literals (numbers, strings, booleans, etc.) and create appropriate AST nodes for each. This ensures that the semantics of the code are preserved during the compilation process.

2. **Memory Allocation Parsing**: The handling of memory allocations (`new`) requires special attention. The parser needs to correctly identify the type being allocated, whether it's a primitive type or a user-defined class, and handle optional array sizes or constructor arguments. This complexity arises from the need to support both dynamic and static memory allocation mechanisms in Quantum Language.

3. **Error Handling**: Robust error handling is implemented to manage unexpected token sequences. For example, if the parser encounters a `new` keyword followed by an invalid type name, it throws a `ParseError`. This helps maintain the integrity of the AST and prevents further processing errors.

4. **Token Consumption**: After interpreting a literal, the parser consumes the token to move on to the next part of the expression. This ensures that the correct sequence of tokens is processed and avoids potential lookahead issues.

## Major Classes/Functions Overview

### Class: `Parser`
- **Function: `parsePrimary()`**
  - **Purpose**: Parses primary expressions, which include literals and memory allocations.
  - **Process**:
    - Checks the current token type and processes accordingly.
    - For numbers, converts them to either integer or floating-point values based on the presence of a hexadecimal prefix or decimal notation.
    - For strings, extracts the string value and creates a `StringLiteral` node.
    - For booleans (`true` or `false`), creates a `BoolLiteral` node.
    - For `nil`, creates a `NilLiteral` node.
    - For identifiers (`this`), creates an `Identifier` node representing the current object.
    - For memory allocations (`new`), parses the type name and optional arguments or array sizes, creating a `NewExpr` node.

### Function: `isCTypeKeyword()`
- **Purpose**: Determines if a given token type corresponds to a C/C++ primitive type keyword (e.g., `int`, `char`, `float`).
- **Why**: This function is necessary to support both Quantum Language-specific types and standard C/C++ types when parsing memory allocations.

### Function: `parseArgList()`
- **Purpose**: Parses a list of arguments enclosed in parentheses, typically used in memory allocation expressions.
- **Process**:
  - Consumes the opening parenthesis.
  - Recursively parses each argument until the closing parenthesis is encountered.
  - Returns a vector of AST nodes representing the parsed arguments.

## Tradeoffs

1. **Flexibility vs. Complexity**: Supporting both quantum language-specific types and standard C/C++ types adds complexity to the parser but enhances its flexibility and interoperability.

2. **Performance vs. Error Handling**: More rigorous error checking can improve code quality but may slightly impact performance. Balancing these aspects is crucial for maintaining both robustness and efficiency.

3. **Readability vs. Maintainability**: While the implementation aims for clarity, ensuring that the code remains maintainable over time is equally important. This involves keeping the code modular, well-documented, and free of unnecessary complexity.

By addressing these design decisions and tradeoffs, `ParserLiterals.cpp` provides a solid foundation for the Quantum Language compiler's parsing capabilities, enabling accurate interpretation and conversion of literal values into AST nodes.