# ParserLiterals.cpp

## Role in Compiler Pipeline

`ParserLiterals.cpp` is a crucial component of the parser stage within the Quantum Language compiler. Its primary function is to handle the parsing of various literal types, such as numbers, strings, booleans, `nil`, identifiers (`this`), and memory allocations (`new`). By accurately identifying and converting these literals into their respective AST nodes, it ensures that the subsequent stages of compilation can proceed with correct semantic information.

## Key Design Decisions and Why

### Handling Literal Types

The parser needs to differentiate between different literal types like numbers, strings, and booleans. To achieve this, `ParserLiterals.cpp` uses specific token types (`TokenType::NUMBER`, `TokenType::STRING`, etc.) to identify each literal. Once identified, the corresponding literal node is created and returned. For instance, when encountering a number token, it checks for optional suffixes like "f", "F", "l", "L", "d", "D", or "u", "U" to determine the numeric type and then skips them before creating the AST node.

### Memory Allocation Parsing

Memory allocation literals (`new`) require special handling due to their complex syntax. The parser must correctly interpret whether the allocation is for a single object or an array. If an array is detected, it parses the expression inside the brackets to determine the size of the array. Additionally, the parser accounts for pointer qualifiers following the type name, ensuring that they do not affect the array allocation logic.

## Major Classes/Functions Overview

### `Parser::parsePrimary()`

This function serves as the entry point for parsing primary expressions, which include literals. Depending on the current token type, it calls appropriate helper functions to parse the literal and create the corresponding AST node. For example:
- Numbers are parsed using `std::stod` or `std::stoull` based on the presence of hexadecimal prefixes.
- Strings are simply extracted from the token value.
- Booleans (`true` and `false`) are converted to boolean literals.
- `nil` is represented as a null literal.
- Identifiers (`this`) are treated as special identifiers.
- Memory allocations (`new`) are handled by calling `parseNew()`.

### `Parser::parseNew()`

This function specifically parses memory allocation literals (`new`). It determines whether the allocation is for a single object or an array by checking for square brackets. If an array is detected, it parses the expression inside the brackets to determine the size of the array. The function also handles pointer qualifiers following the type name.

## Tradeoffs

### Accuracy vs Complexity

Ensuring accurate parsing of all possible literal types requires a significant amount of complexity in the code. While this approach guarantees that all valid literals are correctly parsed, it may lead to increased maintenance costs and potential performance overhead due to the numerous conditional checks.

### Flexibility vs Consistency

The parser's ability to handle both C++ primitive type keywords and identifier class names adds flexibility but could introduce inconsistencies if not properly managed. Ensuring that the same literal type is consistently represented across different contexts requires careful design and testing.

By addressing these tradeoffs, `ParserLiterals.cpp` provides a robust solution for parsing literals in the Quantum Language compiler, enabling the subsequent stages of compilation to work with accurate and consistent semantic information.