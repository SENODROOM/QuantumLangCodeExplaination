# ParserLiterals.cpp

## Role in Compiler Pipeline

`ParserLiterals.cpp` is an essential part of the parser component within the Quantum Language compiler. It handles the parsing of various literal types including numbers, strings, booleans, `nil`, identifiers (`this`), and memory allocations (`new`). This module ensures accurate identification and conversion of these literals into their respective AST nodes.

## Key Design Decisions and WHY

1. **Handling Different Literal Types**:
   - The function `parsePrimary()` is designed to recognize and parse different types of literals such as numbers, strings, booleans, `nil`, and identifiers. Each type has its own specific handling logic, ensuring that they are parsed correctly according to their syntax rules.

2. **Memory Allocation Handling**:
   - Memory allocations using `new` are handled with special attention. The parser checks for the presence of `new` followed by a type name or a C++ primitive type keyword. If an array allocation is detected (`new int[n]`), it parses the size expression and constructs an appropriate AST node. For heap allocations (`new int(100)` or `new ClassName(args)`), it parses the argument list and constructs a `NewExpr` node, which represents the allocation of a new object on the heap.

3. **Flexibility with Type Names**:
   - The parser allows for flexibility in specifying type names. It can accept both identifier class names and C++ primitive type keywords, making it versatile for different programming paradigms and language features.

4. **Error Handling**:
   - Robust error handling is implemented to manage unexpected inputs gracefully. If the parser encounters an invalid token sequence during the parsing process, it throws a `ParseError` with a descriptive message indicating the expected input and the actual input encountered.

## Major Classes/Functions Overview

- **Class**: `Parser`
  - **Function**: `parsePrimary()`
    - **Role**: Parses primary expressions, which include literals like numbers, strings, booleans, `nil`, identifiers (`this`), and memory allocations (`new`).
    - **Parameters**: None
    - **Returns**: An `ASTNodePtr` representing the parsed literal or memory allocation expression.
    - **Logic**:
      - Checks the current token type and processes accordingly.
      - Handles number literals, converting them to `double`.
      - Parses string literals, removing surrounding quotes.
      - Processes boolean literals (`true` and `false`).
      - Identifies `nil` literals.
      - Recognizes identifiers (`this`) and converts them to an `Identifier` AST node.
      - Handles memory allocations (`new`), distinguishing between array and heap allocations, and parsing type names and arguments.

## Tradeoffs

- **Complexity vs. Flexibility**: While providing flexibility in parsing different types of literals and memory allocations, the implementation adds complexity to the parser. This complexity arises from the need to handle multiple token sequences and construct various types of AST nodes.
  
- **Performance vs. Error Handling**: The parser includes comprehensive error handling mechanisms to ensure robustness against invalid input. However, this additional error checking may introduce some performance overhead compared to a simpler parser without extensive error handling.

- **Readability vs. Maintainability**: The code structure aims to balance readability and maintainability. By clearly separating the logic for different literal types and memory allocation scenarios, the code remains organized and easier to understand. This organization also facilitates future maintenance and updates to the parser.

Overall, `ParserLiterals.cpp` plays a crucial role in accurately parsing literal values and memory allocations within the Quantum Language compiler. Its design choices reflect a balance between flexibility, performance, and readability, ensuring that the parser can effectively handle a wide range of input scenarios while maintaining robustness and reliability.