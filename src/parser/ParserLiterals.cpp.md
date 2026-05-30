# ParserLiterals.cpp

## Role in Compiler Pipeline

`ParserLiterals.cpp` plays a crucial role in the Quantum Language compiler's parsing phase. It interprets and converts various literal values such as numbers, strings, booleans, `nil`, identifiers (`this`), and memory allocations (`new`) into their corresponding Abstract Syntax Tree (AST) nodes. This conversion is essential for accurately representing the structure and semantics of the source code within the AST, enabling subsequent phases of compilation to proceed correctly.

## Key Design Decisions and Why

### Literal Handling
The parser handles different types of literals:
- **Numbers**: Converts numeric strings into `double` values, supporting both decimal and hexadecimal formats.
- **Strings**: Directly captures string literals and stores them as `std::string`.
- **Booleans**: Parses `true` and `false` keywords and converts them into boolean literals.
- **Nil**: Represents the `nil` keyword as a special literal indicating the absence of a value.
- **Identifiers (`this`)**: Interprets the `this` keyword as an identifier referring to the current object or context.
- **Memory Allocations (`new`)**: Handles dynamic memory allocation using the `new` keyword, which can be used to create instances of classes or arrays of basic types.

### Trade-offs
- **Flexibility vs. Complexity**: The parser must handle both C++ primitive type keywords and custom class names, adding complexity but ensuring broad compatibility.
- **Performance vs. Correctness**: Efficiently parsing literals without sacrificing correctness is critical. Using `std::stod` and `std::stoull` ensures performance while maintaining accuracy in number conversions.
- **Readability vs. Usability**: The parser's ability to handle multiple literal types improves usability, making it easier for developers to write and understand the source code. However, this also increases the complexity of the parser's logic.

## Major Classes/Functions Overview

### Class: `Parser`
- **Function: `parsePrimary()`**
  - **Purpose**: Parses primary expressions, including literals and memory allocations.
  - **Logic**:
    - Checks the current token type and consumes it accordingly.
    - For numbers, it determines whether the number is hexadecimal or decimal and converts it to a `double`.
    - For strings, it directly captures the string value.
    - For booleans, it parses `true` and `false` keywords.
    - For `nil`, it represents the absence of a value.
    - For identifiers (`this`), it interprets the `this` keyword as an identifier.
    - For memory allocations (`new`), it handles both creating instances of classes and allocating arrays of basic types.
  - **Trade-offs**:
    - **Complexity**: The function must manage multiple cases, increasing its complexity.
    - **Correctness**: Ensuring accurate parsing of all literal types is critical for maintaining the integrity of the AST.
    - **Usability**: Supporting both C++ primitive types and custom class names enhances usability but adds to the complexity of the parser.

### Function: `parseExpr()`
- **Purpose**: Parses expressions that may follow a literal when defining an array size with `new`.
- **Logic**:
  - Consumes the opening `[` token.
  - Parses the expression inside the brackets, which defines the size of the array.
  - Expects the closing `]` token.
- **Trade-offs**:
  - **Readability**: Clear separation between literal parsing and expression parsing improves readability.
  - **Performance**: Efficient parsing of expressions ensures good performance during compilation.

### Function: `parseArgList()`
- **Purpose**: Parses argument lists for memory allocations using the `new` keyword.
- **Logic**:
  - Consumes the opening `(` token.
  - Parses individual arguments until the closing `)` token is encountered.
- **Trade-offs**:
  - **Complexity**: Managing nested parentheses and handling varying numbers of arguments increases complexity.
  - **Correctness**: Accurate parsing of argument lists ensures proper construction of objects or arrays during compilation.

## Conclusion

`ParserLiterals.cpp` is a vital part of the Quantum Language compiler, responsible for interpreting and converting literal values into AST nodes. By carefully managing multiple literal types and optimizing for performance and correctness, the parser ensures that the source code is accurately represented within the AST. This, in turn, facilitates efficient and reliable compilation of the program.