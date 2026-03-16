# `parseShift`

## Purpose

The `parseShift` function is designed to parse shift operations in the Quantum Language compiler. It specifically handles bitwise left (`<<`) and right (`>>`) shifts, which can be part of larger expressions involving addition and subtraction.

## Functionality

The `parseShift` function parses shift operations by first calling `parseAddSub()` to handle the initial expression. It then enters a loop that continues as long as the next token is either a left shift (`TokenType::LSHIFT`) or a right shift (`TokenType::RSHIFT`). Inside the loop, it consumes the shift operator and calls `parseAddSub()` again to get the right-hand side of the shift operation. The resulting binary expression is wrapped in an `ASTNode` and added back into the `left` variable. This process repeats until there are no more shift operators in the sequence.

### Parameters/Return Value

- **Parameters**: None
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed shift expression.

### Edge Cases

- If there are no shift operators after the initial expression, the function returns the result of `parseAddSub()`.
- If the next token after a shift operator is an assignment operator (`=`), the loop breaks early to prevent parsing of compound assignment operations like `<<=` or `>>=`, which are handled separately by `parseAssignment`.

### Interactions with Other Components

The `parseShift` function interacts with other components through the use of helper functions such as `consume()`, `check()`, and `current()`. These functions are used to traverse and analyze the sequence of tokens provided by the parser. The `ASTNode` class is also utilized to construct the abstract syntax tree (AST) representation of the parsed shift expression, facilitating further processing during compilation.