# `parseBitwise` Function

## Purpose
The `parseBitwise` function is designed to parse expressions involving bitwise operations like AND (`&`), OR (`|`), and XOR (`^`). It leverages an existing parser infrastructure for equality expressions and extends it to handle these additional bitwise operators.

## Parameters/Return Value
- **Parameters**: None explicitly listed in the provided code snippet.
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed bitwise expression.

## How It Works
1. **Initial Parsing**: The function starts by calling `parseEquality()`, which parses the initial part of the expression up to the first equality operator (`==` or `!=`). This result is stored in the variable `left`.

2. **Loop for Bitwise Operations**: The function then enters a loop that continues as long as the next token is one of the bitwise operators (`TokenType::BIT_AND`, `TokenType::BIT_OR`, or `TokenType::BIT_XOR`).

3. **Consuming Operator**: Inside the loop, the function consumes the current bitwise operator token using `consume()` and retrieves its value (e.g., `"&"`, `"|"`, `"^"`).

4. **Recursive Parsing**: The function recursively calls `parseEquality()` again to parse the right-hand side of the bitwise operation. This result is stored in the variable `right`.

5. **Building AST Node**: After parsing both sides, the function creates a new `ASTNode` containing a `BinaryExpr` object. This `BinaryExpr` object holds the operator value and pointers to the left and right sub-expressions. The line number of the current token is also passed to the `ASTNode` constructor.

6. **Updating Left Expression**: The newly created `ASTNode` becomes the new `left` expression, and the process repeats with the updated `left` until there are no more bitwise operators in the sequence.

7. **Returning Final Expression**: Once the loop ends, the function returns the final `left` expression, which now represents the entire bitwise expression tree.

## Edge Cases
- **No Bitwise Operators**: If the input expression contains no bitwise operators after the initial call to `parseEquality()`, the function will simply return the result of `parseEquality()`.
- **Nested Expressions**: The function can handle nested bitwise expressions due to its recursive nature. For example, `a & b | c ^ d` would be correctly parsed as `(a & (b | (c ^ d)))`.
- **Invalid Tokens**: If the next token is not a valid bitwise operator, the loop condition will fail, and the function will return the initial parsed expression without any modifications.

## Interactions with Other Components
- **Parser Infrastructure**: The `parseBitwise` function relies on the existing parser infrastructure, particularly the `parseEquality()` function, to handle the base case of equality expressions.
- **Token Stream**: The function uses the global token stream (`current()`, `consume()`) to read and consume tokens. These tokens represent the different parts of the expression being parsed.
- **AST Construction**: The function constructs an Abstract Syntax Tree (AST) using `std::make_unique<ASTNode>` and `BinaryExpr`. This AST represents the hierarchical structure of the bitwise expression, making it easier to evaluate and manipulate programmatically.

Overall, the `parseBitwise` function provides a robust mechanism for parsing complex bitwise expressions within the Quantum Language compiler, ensuring that the resulting AST accurately reflects the intended computation.