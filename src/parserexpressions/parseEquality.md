# `parseEquality` Function

## Purpose

The `parseEquality` function in the Quantum Language Compiler is designed to parse equality expressions from the source code. It supports both loose (`==`, `!=`) and strict (`===`, `!==`) equality comparisons. However, unlike statically typed languages, Quantum is dynamically typed, so the function internally treats strict equality (`===`) as loose equality (`==`) and strict inequality (`!==`) as loose inequality (`!=`). This behavior simplifies the implementation of the parser by reducing the need to handle different types of equality operators explicitly.

## Parameters

- None directly specified in the function signature, but it implicitly relies on global state managed by the parser, such as the current token being processed and methods like `consume()` which advances the token stream.

## Return Value

- Returns a unique pointer to an `ASTNode` representing the parsed equality expression. If there are no equality operators present after the initial comparison, it returns the result of `parseComparison()`, which could be any type of comparison expression.

## Edge Cases

1. **No Equality Operators**: If the initial call to `parseComparison()` results in a valid expression and there are no subsequent equality or inequality tokens, the function simply returns that expression without wrapping it in a binary expression node.
2. **Mixed Types**: Since Quantum is dynamically typed, the function does not enforce type checking during equality comparisons. This means that expressions like `5 == "5"` can be parsed and evaluated correctly.
3. **Nested Expressions**: The function can handle nested equality expressions, where one comparison is part of another. For example, `(a == b) && (c == d)` would be parsed as a series of binary operations.

## Interactions with Other Components

- **Tokenizer**: The function uses the tokenizer to fetch and process tokens from the input source code.
- **ASTBuilder**: Internally, the function constructs an abstract syntax tree (AST) using the `ASTNode` class. Each equality or inequality operation is represented as a `BinaryExpr` node within the AST.
- **Error Handling**: The function may interact with error handling mechanisms to report issues if unexpected tokens are encountered during parsing.

## Implementation Details

The function begins by calling `parseComparison()` to get the left-hand side of the equality expression. It then enters a loop that continues as long as the next token is an equality operator (`TokenType::EQ`, `TokenType::NEQ`, `TokenType::STRICT_EQ`, or `TokenType::STRICT_NEQ`). Inside the loop:

- The current line number is recorded.
- The equality or inequality operator token is consumed.
- Depending on whether the operator is strict (`===` or `!==`), it converts the operator to its corresponding loose form (`==` or `!=`).
- Another call to `parseComparison()` retrieves the right-hand side of the expression.
- A new `BinaryExpr` node is created with the operator string, the left-hand side, and the right-hand side, and wrapped in an `ASTNode`.
- The loop repeats until no more equality or inequality operators are found.

Finally, the function returns the root of the constructed AST, which represents the entire equality expression.

This design ensures that the parser can efficiently handle various forms of equality expressions while maintaining compatibility with Quantum's dynamic typing system.