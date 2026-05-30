# `parseShift` Function

## Purpose
The `parseShift` function is designed to parse shift expressions in the Quantum Language compiler. Shift expressions include bitwise left (`<<`) and right (`>>`) shifts. This function ensures that the correct order of operations is maintained during parsing, particularly when dealing with compound assignment operators like `<<=` and `>>=`.

## Parameters
- None explicitly listed in the code snippet provided.

## Return Value
- Returns a unique pointer to an `ASTNode` representing the parsed shift expression.

## Detailed Explanation
The `parseShift` function starts by calling `parseAddSub()` to parse the left-hand side of the shift expression. It then enters a loop that continues as long as the next token is either a left shift (`TokenType::LSHIFT`) or a right shift (`TokenType::RSHIFT`). 

Inside the loop:
1. The function checks if the next token is followed by an assignment operator (`TokenType::ASSIGN`). If so, it breaks out of the loop because these are handled separately by the `parseAssignment` function.
2. It records the current line number using `current().line`.
3. The function consumes the shift operator token using `consume().value`, which retrieves the actual string representation of the operator.
4. Another call to `parseAddSub()` parses the right-hand side of the shift expression.
5. An `ASTNode` is created with a `BinaryExpr` containing the shift operator, the left-hand side, and the right-hand side. This node represents the entire shift expression.
6. The new node is assigned back to `left`, preparing it for further processing in subsequent iterations of the loop.

Once the loop exits, the function returns the final `ASTNode` representing the parsed shift expression.

## Edge Cases
- **No Shift Operators**: If there are no shift operators following the initial call to `parseAddSub()`, the function will simply return the result of `parseAddSub()`.
- **Compound Assignment Operators**: The function correctly identifies and handles compound assignment operators like `<<=` and `>>=`, breaking out of the loop when such an operator is encountered.

## Interactions with Other Components
- **`parseAddSub()`**: This function is called twice within `parseShift()`. The first call parses the left-hand side of the shift expression, and the second call parses the right-hand side.
- **`parseAssignment()`**: The function breaks out of its loop when it encounters a shift operator followed by an assignment operator, indicating that the compound assignment should be parsed by `parseAssignment()` instead.
- **Token Consumption**: The function uses `consume()` to retrieve the shift operator token, ensuring that the parser moves forward after each operation.

This design allows `parseShift` to handle both simple and complex shift expressions efficiently, maintaining the integrity of the abstract syntax tree (AST).