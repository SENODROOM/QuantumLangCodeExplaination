# ParserExpressions

The `ParserExpressions` component is a crucial module within the Quantum Language compiler, designed to parse and interpret various types of expressions encountered in quantum programs. This ensures that quantum code remains syntactically correct and semantically meaningful throughout its execution.

## Overview

The `ParserExpressions` module encompasses multiple functions, each dedicated to handling different aspects of expression parsing:

- **parseAssignment**: Parses assignment statements, assigning values to variables.
- **parseOr**: Handles logical OR operations between two expressions.
- **parseAnd**: Handles logical AND operations between two expressions.
- **parseBitwise**: Processes bitwise operations such as AND, OR, XOR, NOT, LEFT SHIFT, and RIGHT SHIFT.
- **parseEquality**: Evaluates equality comparisons between two expressions.
- **parseComparison**: Performs comparison operations like greater than, less than, etc.
- **parseShift**: Manages shift operations on integers.
- **parseAddSub**: Handles addition and subtraction operations.
- **parseMulDiv**: Processes multiplication and division operations.
- **parsePower**: Evaluates power operations using exponentiation.
- **parseUnary**: Parses unary operations such as negation and increment/decrement.

These functions work together to form a comprehensive expression parsing system that can handle complex quantum program logic.

## File Structure

Each function is implemented in its respective file within this directory:

- `assignment.cpp`: Contains the implementation of `parseAssignment`.
- `logical_or.cpp`: Implements the `parseOr` function.
- `logical_and.cpp`: Implements the `parseAnd` function.
- `bitwise_operations.cpp`: Includes implementations for bitwise operations (`parseBitwise`).
- `equality_comparison.cpp`: Contains the `parseEquality` function.
- `comparison_operations.cpp`: Implements the `parseComparison` function.
- `shift_operations.cpp`: Manages the `parseShift` function.
- `add_subtract.cpp`: Handles the `parseAddSub` function.
- `multiplication_division.cpp`: Implements the `parseMulDiv` function.
- `power_operation.cpp`: Contains the `parsePower` function.
- `unary_operations.cpp`: Manages the `parseUnary` function.

## Overall Flow

1. **Input Parsing**: The input quantum program is read and tokenized into individual elements.
2. **Expression Identification**: The tokenizer identifies potential expressions based on the tokens.
3. **Function Invocation**: Depending on the identified expression type, the corresponding function from the `ParserExpressions` module is invoked.
4. **Expression Evaluation**: Each function evaluates the specific expression type, performing necessary computations or logical checks.
5. **Output Generation**: The evaluated result is used to generate further instructions or outputs, ensuring the quantum program executes correctly.

By following this structured approach, the `ParserExpressions` component efficiently handles the parsing and interpretation of expressions, enabling robust and accurate quantum program compilation and execution.