# `parseShift`

## Purpose

The `parseShift` function is designed to parse shift operations in the Quantum Language compiler. It specifically handles bitwise left (`<<`) and right (`>>`) shifts, which can be part of larger expressions involving addition and subtraction.

## Functionality

### What it Does

- The function starts by parsing an expression using the `parseAddSub()` method. This initial expression could be any valid arithmetic expression that might include shift operations.
- It then enters a loop where it checks for either a left shift (`TokenType::LSHIFT`) or a right shift (`TokenType::RSHIFT`) token.
- If a shift operation is found, the function consumes the token and parses another arithmetic expression using `parseAddSub()`. This second expression will be the operand on the right side of the shift operation.
- A new `ASTNode` representing a binary expression is created, with the shift operator, the parsed left expression, and the parsed right expression. The line number of the current token is used to set the line number of the new node.
- The newly created `ASTNode` becomes the new left expression for the next iteration of the loop, allowing for consecutive shift operations within the same expression.
- The loop continues until no more shift operators are found or a compound assignment operator (`>>=` or `<<=`) is encountered.

### Why it Works This Way

This design allows for the parsing of complex expressions involving multiple shift operations. By repeatedly calling `parseAddSub()` and combining the results with shift operators, the function can handle nested shift operations correctly. Additionally, the loop structure ensures that all shift operations are processed before moving on to other types of expressions.

The decision not to consume the `>>=` or `<<=` tokens immediately is crucial because these are considered compound assignment operators in many programming languages. By breaking out of the loop when such a token is encountered, the function allows for proper handling of these operators by the `parseAssignment()` method, which can then combine them with the shift operation into a single AST node.

## Parameters/Return Value

- **Parameters**: None explicitly listed in the provided code snippet, but implicitly uses global variables like `tokens` and `pos`.
- **Return Value**: Returns a unique pointer to an `ASTNode` representing the parsed expression, including any shift operations.

## Edge Cases

- **No Shift Operators**: If there are no shift operators in the input, the function simply returns the result of `parseAddSub()`, which could be a standalone operand or a more complex expression.
- **Consecutive Shift Operations**: Multiple consecutive shift operations are handled gracefully, with each operation being combined into a single AST node.
- **Compound Assignment Operators**: When encountering a compound assignment operator (`>>=` or `<<=`), the loop breaks, allowing the `parseAssignment()` method to handle the combination of the shift operation and the assignment.

## Interactions with Other Components

- **`parseAddSub()` Method**: This method is called at the beginning of `parseShift()` and also inside the loop whenever a shift operator is found. It is responsible for parsing expressions involving addition, subtraction, and shift operations.
- **`consume()` Method**: This method is used to consume the current token in the input stream. In the context of `parseShift()`, it consumes the shift operator token (`<<` or `>>`) and moves to the next token.
- **`check(TokenType type)` Method**: This method checks if the current token matches the specified token type without consuming it. It is used to determine whether a shift operator is present in the input stream.
- **`std::make_unique<ASTNode>()` Function**: This function creates a new unique pointer to an `ASTNode` object, which represents a node in the abstract syntax tree (AST). The `BinaryExpr` constructor is used to create a new binary expression node containing the shift operator, the left expression, and the right expression.
- **Error Handling**: While not shown in the provided code snippet, the function likely includes error handling mechanisms to manage unexpected tokens or malformed expressions.

In summary, the `parseShift` function efficiently parses expressions involving bitwise shift operations, ensuring correct handling of both individual and consecutive shift operations, as well as interaction with compound assignment operators through its interaction with the `parseAssignment()` method.