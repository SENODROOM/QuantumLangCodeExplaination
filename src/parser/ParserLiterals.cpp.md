# ParserLiterals.cpp

## Role in Compiler Pipeline

`ParserLiterals.cpp` is an essential component of the Quantum Language compiler's parser stage. Its primary function is to handle the parsing of various literal types, such as numbers, strings, booleans, `nil`, identifiers (`this`), and memory allocations (`new`). This stage transforms these literals into their respective Abstract Syntax Tree (AST) nodes, facilitating further compilation steps.

## Key Design Decisions and WHY

### Handling Literal Types
The parser distinguishes between different literal types based on their token type. For instance, it recognizes numbers, strings, booleans, and special values like `nil`. Each type is parsed using specific functions that convert the token value into its AST representation.

- **Numbers**: The parser checks if the number is hexadecimal or decimal and uses appropriate standard library functions (`std::stoull` for unsigned long long, `std::stod` for double) to convert the string representation into a numeric value. It also handles optional suffixes like `f`, `F`, `l`, `L`, `d`, `D`, `u`, and `U` which specify the floating-point format.

- **Strings**: Strings are straightforwardly converted from the token value to a `StringLiteral` node. The parser consumes the token and constructs the AST node with the string content.

- **Booleans**: Booleans (`true` and `false`) are directly mapped to `BoolLiteral` nodes. The parser consumes the token and creates the appropriate boolean literal.

- **Nil**: The `nil` keyword is parsed into a `NilLiteral` node. This represents a null or undefined value in the AST.

- **Identifiers (`this`)**: The identifier `this` is handled similarly to other identifiers but specifically maps to `"self"` in the AST. This simplifies the handling of object references within the language.

- **Memory Allocations (`new`)**: The parser supports dynamic memory allocation using the `new` keyword. It can handle arrays (`new int[n]`) and objects (`new ClassName(args)`). The parser ensures that the allocated memory is correctly represented as a pointer in the AST.

### Tradeoffs

#### Complexity vs. Usability
While adding support for more complex literal types like arrays and custom class instances increases the complexity of the parser, it enhances the usability of the Quantum Language by allowing developers to work with dynamic data structures easily.

#### Error Handling
The parser includes robust error handling mechanisms to manage unexpected input gracefully. For example, if the parser encounters an invalid type name after `new`, it throws a `ParseError` indicating the expected type. This helps in identifying issues early during the compilation process.

#### Performance Considerations
Handling different literal types efficiently requires careful consideration of performance. For instance, converting large numbers from string to numeric types should be done quickly without unnecessary overhead. Additionally, managing memory allocations and ensuring they are correctly represented in the AST can impact performance, especially when dealing with large arrays or complex objects.

## Major Classes/Functions Overview

### Class: Parser
The `Parser` class is central to the parsing process. It contains methods to parse various components of the source code, including literals. The `parsePrimary` method is particularly important as it handles the parsing of primary expressions, which include literals.

### Function: parsePrimary
This function is responsible for parsing primary expressions, which start with a literal. Depending on the token type, it calls specialized functions to parse each literal type:

- **parseNumberLiteral**: Parses numeric literals, handling both decimal and hexadecimal formats.
- **parseStringLiteral**: Converts string tokens into `StringLiteral` nodes.
- **parseBoolLiteral**: Maps boolean tokens (`true` and `false`) to `BoolLiteral` nodes.
- **parseNilLiteral**: Handles the `nil` keyword by creating a `NilLiteral` node.
- **parseThisLiteral**: Transforms the `this` identifier into a `Identifier{"self"}` node.
- **parseNewExpression**: Parses memory allocation expressions (`new`), supporting both array and object allocations.

Each of these functions contributes to building a comprehensive and accurate AST, which is crucial for subsequent stages of the compiler.

By understanding these roles and decisions, developers can better appreciate how `ParserLiterals.cpp` integrates into the broader quantum language compiler architecture, enhancing its functionality and reliability.