# `parseShift` Function

## Purpose
The `parseShift` function is designed to parse shift expressions in the Quantum Language compiler. Shift expressions include bitwise left (`<<`) and right (`>>`) shifts. This function ensures that the correct order of operations is maintained during parsing, particularly when dealing with compound assignment operators like `<<=` and `>>=`.

## Parameters/Return Value
- **Parameters**:
  - None explicitly listed in the code snippet provided.
  
- **Return Value**:
  - Returns a unique pointer to an `ASTNode` representing the parsed shift expression.

## How It Works
The function starts by calling `parseAddSub()` to parse the left-hand side of the shift expression. It then enters a loop that continues as long as the next token is either a left shift (`<<`) or a right shift (`>>`). Inside the loop:

1. The function checks if the next token after the current one is an assignment operator (`=`). If so, it breaks out of the loop because these are handled as compound assignment operators by `parseAssignment`.
2. It records the current line number using `current().line`.
3. The function consumes the current token (which is either `<<` or `>>`) and stores its value.
4. Another call to `parseAddSub()` parses the right-hand side of the shift expression.
5. A new `ASTNode` is created with a `BinaryExpr` containing the operation type, the left-hand side, and the right-hand side. This node represents the parsed shift expression.
6. The loop repeats until there are no more shift operators.

## Edge Cases
- **No Shift Operators**: If the input sequence does not contain any shift operators, the function will simply return the result of `parseAddSub()`, which could be a constant, variable, or another expression.
- **Compound Assignment Operators**: The function correctly identifies and handles compound assignment operators like `<<=` and `>>=`, ensuring they are not mistakenly interpreted as separate shift and assignment operations.

## Interactions With Other Components
- **`parseAddSub()`**: This function is called twice within `parseShift`. The first call parses the left-hand side of the shift expression, and the second call parses the right-hand side.
- **`consume()`**: This function is used to consume the current token, which is either `<<` or `>>`.
- **`check(TokenType::ASSIGN)`**: This function checks if the next token is an assignment operator, which helps in distinguishing between simple shift operations and compound assignment operations.
- **`std::make_unique<ASTNode>()`**: This is used to create a new `ASTNode` representing the binary expression formed by the shift operation.

Overall, `parseShift` plays a crucial role in maintaining the correct syntax and semantics of shift expressions in the Quantum Language compiler by leveraging existing parsing functions and handling special cases appropriately.