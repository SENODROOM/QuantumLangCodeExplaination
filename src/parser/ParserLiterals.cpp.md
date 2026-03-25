# ParserLiterals.cpp

## Role in Compiler Pipeline

`ParserLiterals.cpp` is part of the parser component of the Quantum Language compiler. Its primary role is to handle the parsing of literal values such as numbers, strings, booleans, `nil`, identifiers (`this`), and memory allocations (`new`). This module ensures that these literals are correctly identified and converted into their corresponding Abstract Syntax Tree (AST) nodes, which are essential building blocks for further compilation stages.

## Key Design Decisions and WHY

### Handling Different Literal Types

The parser needs to distinguish between different types of literals like numbers, strings, booleans, etc. Each type requires specific handling due to its syntax and potential data representation. For example:
- **Numbers**: The function checks if the number is hexadecimal or decimal and converts it accordingly.
- **Strings**: Strings need to be properly escaped and handled as sequences of characters.
- **Booleans**: There are separate token types for `true` and `false`.

### Memory Allocation with `new`

Handling memory allocation (`new`) introduces complexity because it can result in either a single object or an array, and it may include type qualifiers like pointers. The parser must correctly interpret these constructs and generate appropriate AST nodes:
- **Single Object**: `new int(100)` results in a heap-allocated integer.
- **Array**: `new int[n]` results in a heap-allocated array of integers.
- **Pointer Array**: `new char*[n]` results in a heap-allocated array of character pointers.

### Trade-offs

#### Simplicity vs. Flexibility

While simplicity might lead to fewer edge cases, flexibility allows the parser to handle a wider range of valid input scenarios. In this case, handling both C++ primitive type keywords and custom class names provides flexibility but increases complexity.

#### Error Handling

Proper error handling is crucial for robust compilers. The parser includes checks for expected token types and throws exceptions when errors occur. This approach ensures that the compiler can gracefully report issues and halt processing if necessary.

#### Performance Considerations

Parsing literals efficiently is important for performance. Using functions like `std::stod` and `std::stoull` directly for conversion minimizes overhead compared to manual parsing logic. Additionally, using `consume()` to advance the token stream avoids unnecessary reiteration over tokens.

## Major Classes/Functions Overview

### Class: ASTNode

`ASTNode` is a base class representing a node in the Abstract Syntax Tree. It has several derived classes for different types of AST nodes, including `NumberLiteral`, `StringLiteral`, `BoolLiteral`, and `NewExpr`. These derived classes encapsulate the specific properties and behaviors of each literal type.

### Function: parsePrimary()

This function is responsible for parsing primary expressions, which include literals. It starts by checking the current token's type and then processes the token based on its type:
- **Numbers**: Converts the token value to a double and creates a `NumberLiteral` node.
- **Strings**: Creates a `StringLiteral` node.
- **Booleans**: Creates a `BoolLiteral` node.
- **Nil**: Creates a `NilLiteral` node.
- **Identifiers (`this`)**: Creates an `Identifier` node with the value `"self"`.
- **Memory Allocation (`new`)**: Handles the creation of `NewExpr` nodes for both single objects and arrays.

### Function: isCTypeKeyword()

This utility function checks if the current token type corresponds to a C++ primitive type keyword (e.g., `int`, `float`, `char`). It is used to determine whether a `new` expression should be treated as allocating a single object or an array.

## Conclusion

`ParserLiterals.cpp` plays a critical role in the Quantum Language compiler by parsing various literal types and memory allocation constructs. Through careful design and implementation, it ensures that these literals are accurately represented in the AST, facilitating subsequent compilation steps. The module's ability to handle complex scenarios like pointer arrays adds significant flexibility, although it does increase the overall complexity of the parser. Proper error handling and efficient use of standard library functions contribute to the reliability and performance of the compiler.