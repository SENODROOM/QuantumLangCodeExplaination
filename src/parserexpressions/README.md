# ParserExpressions

The `ParserExpressions` component is a crucial module within the Quantum Language compiler, designed to parse and interpret expressions encountered in quantum programs. This ensures that quantum code adheres to proper syntax and semantics during its execution.

## Overview

The `ParserExpressions` module consists of several functions that manage different types of expression parsing:

- **parseAssignment**: Parses assignment statements.
- **parseOr**: Handles logical OR operations.
- **parseAnd**: Manages logical AND operations.
- **parseBitwise**: Processes bitwise operations.
- **parseEquality**: Interprets equality comparisons.
- **parseComparison**: Parses comparison operators (e.g., >, <).
- **parseShift**: Handles shift operations.
- **parseAddSub**: Manages addition and subtraction operations.
- **parseMulDiv**: Processes multiplication and division operations.
- **parsePower**: Interprets power operations.
- **parseUnary**: Parses unary operations.

These functions work together to form a comprehensive parsing system for quantum expressions. The overall flow starts with the input quantum program being broken down into tokens. Each token is then passed through the appropriate function based on its type. The results are combined to produce a structured representation of the quantum program, which can be further processed by the compiler's other components.

## Files

Each function is implemented in its own file within this directory, ensuring modularity and ease of maintenance. Here’s a brief overview of the files:

- `parseAssignment.cpp`: Contains the implementation of the `parseAssignment` function.
- `parseOr.cpp`: Implements the `parseOr` function.
- `parseAnd.cpp`: Contains the implementation of the `parseAnd` function.
- `parseBitwise.cpp`: Implements the `parseBitwise` function.
- `parseEquality.cpp`: Contains the implementation of the `parseEquality` function.
- `parseComparison.cpp`: Implements the `parseComparison` function.
- `parseShift.cpp`: Contains the implementation of the `parseShift` function.
- `parseAddSub.cpp`: Implements the `parseAddSub` function.
- `parseMulDiv.cpp`: Contains the implementation of the `parseMulDiv` function.
- `parsePower.cpp`: Implements the `parsePower` function.
- `parseUnary.cpp`: Contains the implementation of the `parseUnary` function.

## Usage

To use the `ParserExpressions` component, include the necessary header files and call the appropriate functions with the parsed tokens from your quantum program. For example:

```cpp
#include "parserexpressions/parseAssignment.h"
#include "parserexpressions/parseOr.h"

// Example usage
std::vector<Token> tokens = tokenizeQuantumProgram("q1 := 0 | q2 := 1");
ExpressionNode* rootNode = parseAssignment(tokens);
if (rootNode) {
    ExpressionNode* orNode = parseOr(*rootNode);
    // Continue processing the AST as needed
}
```

This component is integral to the Quantum Language compiler, providing robust support for expression parsing and interpretation. By leveraging these functions, developers can ensure that their quantum programs are correctly formatted and semantically valid.