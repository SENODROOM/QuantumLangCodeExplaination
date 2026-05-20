# `parseShift` Function

## Purpose
The `parseShift` function is designed to parse shift expressions in the Quantum Language compiler. Shift expressions include bitwise left (`<<`) and right (`>>`) shifts. This function ensures that the correct order of operations is maintained during parsing, particularly when dealing with compound assignment operators like `<<=` and `>>=`, which should be handled separately by the `parseAssignment` function.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed shift expression.

## How It Works
The function starts by calling `parseAddSub()` to parse the left-hand side of the shift expression. It then enters a loop that continues as long as the next token is either a left shift (`TokenType::LSHIFT`) or a right shift (`TokenType::RSHIFT`). Inside the loop:
- The function checks if the next token after the current one is an assignment operator (`TokenType::ASSIGN`). If so, it breaks out of the loop because these compound assignment operators should be handled by `parseAssignment`.
- The function consumes the current token, which will be either `<<` or `>>`, and stores its value.
- It recursively calls `parseAddSub()` again to parse the right-hand side of the shift expression.
- An `ASTNode` is created with a `BinaryExpr` containing the operation type, the left-hand side, and the right-hand side. This node represents the parsed shift expression.
- The line number of the current token is stored in `ln` and used to create the `ASTNode`.

This process repeats until there are no more shift operators to parse, ensuring that all shift operations are correctly nested within their respective sub-expressions.

## Edge Cases
- **No Shift Operators**: If the input does not contain any shift operators, the function simply returns the result of `parseAddSub()`, which could be an integer literal, variable, or another expression.
- **Compound Assignment Operators**: The function correctly identifies and handles compound assignment operators like `<<=` and `>>=`, breaking out of the loop when such an operator is encountered.

## Interactions With Other Components
The `parseShift` function interacts with several other components of the Quantum Language compiler:
- **Tokenizer**: It uses the tokenizer to retrieve and consume tokens based on their type.
- **ASTBuilder**: It constructs an Abstract Syntax Tree (AST) using `ASTNode` objects, where each node represents a part of the expression being parsed.
- **Error Handling**: While not explicitly shown in the code snippet, the function likely includes error handling mechanisms to manage unexpected token types or malformed expressions.

Overall, the `parseShift` function plays a crucial role in parsing shift expressions, ensuring that they are correctly interpreted and represented in the AST.