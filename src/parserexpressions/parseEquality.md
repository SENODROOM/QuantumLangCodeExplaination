# `parseEquality` Function

## Purpose

The `parseEquality` function in the Quantum Language Compiler is designed to parse equality expressions from the source code. It handles both loose (`==`, `!=`) and strict (`===`, `!==`) equality comparisons, treating strict equality (`===`, `!==`) as their non-strict counterparts (`==`, `!=`). This simplification allows the parser to leverage existing comparison parsing logic without duplicating effort.

## Parameters

- None explicitly listed in the provided code snippet; however, it implicitly relies on global state managed by the parser, such as the current token and the ability to consume tokens.

## Return Value

- Returns a unique pointer to an `ASTNode` representing the parsed equality expression. If no equality operators are found after the initial comparison, it returns the result of `parseComparison()` directly.

## How It Works

1. **Initial Comparison**: The function starts by calling `parseComparison()`, which parses the left-hand side of the equality expression.
2. **Loop Through Equality Operators**: It then enters a loop that continues as long as the next token is one of the equality operators (`==`, `!=`, `===`, `!==`).
3. **Consume Operator**: Inside the loop, it consumes the current token using `consume()`. This removes the operator from the input stream.
4. **Operator String Conversion**: Depending on the type of the consumed token:
   - If it's `TokenType::STRICT_EQ`, it converts the operator to `"=="`.
   - If it's `TokenType::STRICT_NEQ`, it converts the operator to `"!="`.
   - For any other equality operator, it uses the original token value.
5. **Right Hand Side Parsing**: After consuming the operator, it calls `parseComparison()` again to parse the right-hand side of the equality expression.
6. **Create ASTNode**: It creates a new `ASTNode` with a `BinaryExpr` containing the operator string, the left-hand side node, and the right-hand side node. The line number of the operator is stored in the `ln` variable.
7. **Update Left Node**: The newly created `ASTNode` becomes the new left-hand side node for the next iteration of the loop or the final result if no more equality operators are found.
8. **Exit Loop**: Once there are no more equality operators, the loop exits, and the function returns the final left-hand side node.

## Edge Cases

- **No Equality Operators**: If the initial call to `parseComparison()` results in a valid expression and there are no subsequent equality operators, the function simply returns the result of `parseComparison()`.
- **Invalid Token Sequence**: If the sequence of tokens encountered is invalid (e.g., expecting an equality operator but finding something else), the behavior depends on how the `consume()` function handles errors.
- **Dynamic Typing Consideration**: Since Quantum is dynamically typed, strict equality (`===`, `!==`) is treated as loose equality (`==`, `!=`). This means that the parsed expressions will not reflect the distinction between strict and non-strict equality, which could lead to unexpected behavior at runtime.

## Interactions With Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the correct sequence of tokens. Each call to `current()` retrieves the current token, and `consume()` advances the tokenizer to the next token.
- **ASTBuilder**: The function constructs an abstract syntax tree (AST) using `ASTNode` objects. These nodes represent different parts of the expression being parsed, including binary expressions.
- **Error Handling**: While not explicitly shown in the provided code, the function likely interacts with error handling mechanisms within the parser to manage unexpected token sequences or other issues during parsing.

This implementation ensures that the parser can handle both types of equality comparisons consistently and efficiently, leveraging its existing capabilities to simplify the process.