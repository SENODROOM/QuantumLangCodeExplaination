# `parseShift` Function

## Purpose
The `parseShift` function is designed to parse shift expressions in the Quantum Language compiler. Shift expressions include bitwise left (`<<`) and right (`>>`) shifts. This function ensures that the correct order of operations is maintained during parsing, particularly handling the precedence of these operators relative to addition and subtraction.

## Parameters
- None explicitly listed in the provided code snippet; however, it implicitly uses global variables such as `tokens`, `current()`, `consume()`, and `pos`.

## Return Value
- Returns a unique pointer to an `ASTNode` representing the parsed shift expression.

## How It Works
The `parseShift` function begins by calling `parseAddSub()` to parse the left-hand side of the shift expression. It then enters a loop that checks for the presence of either a left shift (`TokenType::LSHIFT`) or a right shift (`TokenType::RSHIFT`). If such an operator is found, it consumes the token but does not immediately parse the right-hand side. Instead, it checks if the next token is an assignment operator (`TokenType::ASSIGN`). If so, it breaks out of the loop because these are handled as compound assignment operators by `parseAssignment`.

For each shift operator encountered, the function:
1. Records the line number where the operator appears using `current().line`.
2. Consumes the shift operator token using `consume().value`.
3. Parses the right-hand side of the shift expression by calling `parseAddSub()`.
4. Constructs a new `BinaryExpr` node with the shift operator, the previously parsed left-hand side, and the newly parsed right-hand side.
5. Updates the line number of the new `BinaryExpr` node to match the original line number of the left-hand side.
6. Returns the unique pointer to the constructed `ASTNode`.

This approach allows the function to correctly handle both simple shift expressions and compound assignment operators, ensuring proper order of operations and maintaining the integrity of the abstract syntax tree (AST).

## Edge Cases
- **Empty Input**: The function assumes there is at least one token available. If the input is empty, it may lead to undefined behavior.
- **Invalid Token Sequence**: The function does not validate the sequence of tokens before parsing. Parsing errors can occur if the tokens do not form valid shift expressions.
- **Nested Compound Assignments**: While the function breaks out of the loop upon encountering a compound assignment operator, nested compound assignments within shift expressions are not supported.

## Interactions with Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: Although not shown in the snippet, error handling mechanisms likely exist elsewhere in the compiler to manage parsing errors.
- **Abstract Syntax Tree (AST)**: The function constructs nodes in the AST to represent the parsed shift expressions, which are used for further compilation steps.

Overall, the `parseShift` function plays a crucial role in parsing shift expressions in the Quantum Language compiler, ensuring that they are correctly interpreted and represented in the AST.