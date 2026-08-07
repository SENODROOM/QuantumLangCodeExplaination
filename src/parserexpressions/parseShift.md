# `parseShift` Function

## Purpose
The `parseShift` function is designed to parse shift expressions in the Quantum Language compiler. Shift expressions include bitwise left (`<<`) and right (`>>`) shifts. This function ensures that the correct order of operations is maintained during parsing, particularly when dealing with compound assignment operators like `<<=` and `>>=`.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: A unique pointer to an ASTNode representing the parsed shift expression.

## How It Works
The function starts by calling `parseAddSub()` to parse the left-hand side of the shift expression. It then enters a loop that continues as long as the next token is either a left shift (`TokenType::LSHIFT`) or a right shift (`TokenType::RSHIFT`). Inside the loop:
- The function checks if the next token is an assignment operator (`TokenType::ASSIGN`). If so, it breaks out of the loop because compound assignment operators like `<<=` and `>>=` should be handled separately by `parseAssignment`.
- The function consumes the current token, which represents the shift operator (`<<` or `>>`).
- It calls `parseAddSub()` again to parse the right-hand side of the shift expression.
- A new `ASTNode` is created with a `BinaryExpr` containing the shift operator, the left-hand side, and the right-hand side. This node is then assigned back to `left`, effectively building up the expression tree.

This process repeats until there are no more shift operators in the sequence, ensuring that all shift operations are correctly parsed and ordered.

## Edge Cases
- **No Shift Operators**: If there are no shift operators after the initial call to `parseAddSub()`, the function simply returns the result of that call.
- **Compound Assignment Operators**: The function correctly identifies and handles compound assignment operators like `<<=` and `>>=`, breaking out of the loop when such operators are encountered.

## Interactions With Other Components
- **parseAddSub()**: The `parseShift` function relies on `parseAddSub()` to parse the operands of the shift expressions. This ensures that the operands are correctly evaluated before the shift operation is applied.
- **ASTNode**: The function constructs an `ASTNode` using a `BinaryExpr`, which represents a binary expression in the abstract syntax tree (AST). This node includes the shift operator and its operands, allowing for further processing and analysis during compilation.

Overall, the `parseShift` function plays a crucial role in parsing shift expressions in the Quantum Language compiler, ensuring that they are correctly interpreted and represented in the AST.