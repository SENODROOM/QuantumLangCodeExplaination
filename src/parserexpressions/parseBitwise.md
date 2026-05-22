# `parseBitwise` Function

## Purpose
The `parseBitwise` function is designed to parse expressions that involve bitwise operations such as AND (`&`), OR (`|`), and XOR (`^`). This function builds upon the existing parser infrastructure, which is capable of handling equality expressions, and extends its functionality to include bitwise operations.

## Parameters
- None explicitly listed in the provided code snippet.

## Return Value
- Returns a unique pointer to an ASTNode representing the parsed bitwise expression.

## How It Works
1. **Initialization**: The function starts by calling `parseEquality()` to parse the left-hand side of the bitwise expression. This function returns a unique pointer to an ASTNode representing the equality expression.
   
2. **Loop for Bitwise Operations**: The function then enters a loop that continues as long as the next token is one of the bitwise operators (`TokenType::BIT_AND`, `TokenType::BIT_OR`, or `TokenType::BIT_XOR`).
   - Inside the loop:
     - The line number (`ln`) of the current token is retrieved using `current().line`.
     - The current token is consumed using `consume()`, and its value (`op`) is stored. This value represents the bitwise operator being used.
     - Another call to `parseEquality()` is made to parse the right-hand side of the bitwise expression.
     - A new `ASTNode` is created with a `BinaryExpr` containing the operator (`op`), the previously parsed left-hand side (`left`), and the newly parsed right-hand side (`right`). The line number (`ln`) is also passed to the `ASTNode` constructor.
     - The unique pointer to the new `ASTNode` replaces the previous `left` pointer, effectively chaining the bitwise operations together.

3. **Termination**: Once there are no more bitwise operators following the initial equality expression, the loop terminates, and the final parsed expression (which may be a chain of bitwise operations) is returned.

## Edge Cases
- If the input stream does not contain any bitwise operators after the initial equality expression, the function will simply return the result of parsing the equality expression.
- If the input stream contains multiple consecutive bitwise operators without corresponding operands, the function will raise a syntax error because it expects balanced operands for each bitwise operation.

## Interactions with Other Components
- This function relies on the `parseEquality()` function to handle the base case of equality expressions. These equality expressions can themselves contain bitwise operations, allowing for complex nested expressions.
- The `consume()` function is used to advance the tokenizer beyond the current token, ensuring that subsequent tokens are correctly interpreted as part of the bitwise expression.
- The `check()` function is used to verify the type of the current token before deciding whether to continue parsing the bitwise expression or terminate the loop.

This implementation ensures that the parser can correctly handle expressions involving both equality and bitwise operations, providing a robust foundation for further extensions and optimizations in the quantum language compiler.