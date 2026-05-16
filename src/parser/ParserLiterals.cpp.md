# ParserLiterals.cpp

## Role in Compiler Pipeline

`ParserLiterals.cpp` plays a critical role in the parser stage of the Quantum Language compiler. It is responsible for handling the parsing of various literal types, including numbers, strings, booleans, `nil`, identifiers (`this`), and memory allocations (`new`). The parser converts these literals into their corresponding Abstract Syntax Tree (AST) nodes, ensuring that they can be correctly interpreted during subsequent stages of compilation.

## Key Design Decisions and Why

1. **Handling Different Literal Types**: The parser must identify and convert different literal types into their respective AST nodes. This includes:
   - Numbers: Both integer and floating-point values with optional suffixes like `f`, `F`, `l`, `L`, `d`, `D`, `u`, or `U`.
   - Strings: Enclosed in double quotes.
   - Booleans: Represented by `true` and `false`.
   - `nil`: A special keyword indicating null or empty values.
   - Identifiers (`this`): Used to refer to the current object instance.
   - Memory Allocations (`new`): Used for dynamic memory management in quantum programs.

2. **Flexibility in Type Specification**: To support both user-defined class names and built-in C++ type keywords (e.g., `int`, `char*`), the parser needs to differentiate between them. This ensures compatibility with existing C++ codebases while allowing for custom types in quantum programming.

3. **Array Allocation Handling**: The parser must correctly handle array allocations using the `new` keyword. This involves checking for square brackets `[ ]` and parsing the expression inside to determine the array size. Additionally, it should handle pointer-to-type arrays, treating them similarly to regular arrays at runtime.

4. **Trade-offs Between Simplicity and Completeness**:
   - **Simplicity**: The parser focuses on basic literal types and memory allocations, which are essential components of quantum programs.
   - **Completeness**: While keeping the parser simple, it provides robust support for common literal types and memory management constructs, ensuring that the compiler can handle a wide range of input without compromising performance.

## Major Classes/Functions Overview

### Class: Parser
- **Role**: The main parser class that handles the overall parsing process.
- **Functionality**:
  - `parsePrimary()`: Parses primary expressions, including literals and memory allocations.
  - `parseExpr()`: Parses expressions, including arithmetic operations and function calls.
  - `parseArgList()`: Parses argument lists for functions and constructors.

### Function: parsePrimary()
- **Purpose**: Handles the parsing of primary expressions, focusing on literals and memory allocations.
- **Implementation**:
  - Checks the current token type and processes accordingly.
  - Converts numeric literals into `NumberLiteral` nodes.
  - Converts string literals into `StringLiteral` nodes.
  - Converts boolean literals (`true` and `false`) into `BoolLiteral` nodes.
  - Handles `nil` literals.
  - Processes identifiers (`this`).
  - Manages memory allocations (`new`), including handling array sizes and pointer qualifiers.

## Tradeoffs

- **Performance vs. Complexity**: The parser is designed to be efficient and straightforward, avoiding unnecessary complexity that could impact performance.
- **Compatibility vs. Customization**: While maintaining compatibility with existing C++ codebases, the parser also allows for custom types in quantum programming, providing flexibility but potentially increasing the learning curve for users.
- **Memory Management vs. Ease of Use**: Array allocations are handled explicitly, which might seem more complex compared to automatic memory management in other languages. However, this approach ensures better control and predictability over memory usage in quantum programs.

By carefully balancing these factors, `ParserLiterals.cpp` contributes significantly to the reliability and efficiency of the Quantum Language compiler.