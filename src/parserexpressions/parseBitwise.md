# `parseBitwise` Function

## Purpose
The `parseBitwise` function is designed to parse expressions that involve bitwise operations such as AND (`&`), OR (`|`), and XOR (`^`). This function builds upon the existing parser infrastructure, which is capable of handling equality expressions, and extends it to support more complex logical operations.

## Parameters
- None explicitly listed in the provided code snippet; however, it implicitly relies on global variables or state managed by the parser context.

## Return Value
- Returns a unique pointer to an `ASTNode` representing the parsed bitwise expression. The node contains a `BinaryExpr` object with the operation type and pointers to the left and right operands.

## How It Works
1. **Initial Parsing**: The function starts by calling `parseEquality()` to parse the initial expression. This could be a single variable, a number, or another equality expression.
2. **Loop for Bitwise Operations**: The function enters a loop that continues as long as the next token is one of the bitwise operators (`TokenType::BIT_AND`, `TokenType::BIT_OR`, `TokenType::BIT_XOR`).
3. **Consume Operator**: Inside the loop, the function consumes the current bitwise operator token using `consume()`. This retrieves both the line number (`ln`) and the operator value (`op`).
4. **Recursive Parsing**: After consuming the operator, the function recursively calls `parseEquality()` again to parse the right-hand side of the bitwise expression.
5. **Construct AST Node**: The parsed left and right operands along with the consumed operator are used to construct a new `BinaryExpr` object. This object represents the bitwise operation. An `ASTNode` containing this `BinaryExpr` is then created and returned.
6. **Edge Case Handling**: If there are no more bitwise operators after the initial parsing, the function simply returns the initially parsed expression wrapped in an `ASTNode`.

## Edge Cases
- **No Bitwise Operators**: If the expression does not contain any bitwise operators (`&`, `|`, `^`), the function will return the result of `parseEquality()` directly.
- **Nested Expressions**: The function can handle nested bitwise expressions due to its recursive nature. For example, `a & b | c ^ d` would be correctly parsed into a tree structure reflecting the order of operations.

## Interactions with Other Components
- **Tokenizer**: The function interacts with the tokenizer to retrieve tokens and determine their types (`TokenType::BIT_AND`, etc.).
- **Error Handling**: Although not shown in the snippet, the parser infrastructure typically includes error handling mechanisms that would be triggered if invalid tokens or syntax are encountered during parsing.
- **Precedence Handling**: The function implicitly handles operator precedence through its recursive nature. Higher precedence operators are evaluated first before lower ones within the same expression.

This implementation ensures that the parser can correctly interpret and build abstract syntax trees (ASTs) for expressions involving bitwise operations, leveraging the existing capabilities of the parser for simpler equality expressions.