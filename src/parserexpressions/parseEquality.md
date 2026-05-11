# `parseEquality` Function

## Purpose

The `parseEquality` function in the Quantum Language Compiler is designed to parse equality expressions from the source code. It handles both loose (`==`, `!=`) and strict (`===`, `!==`) equality comparisons, treating strict equality (`===`, `!==`) as their non-strict counterparts (`==`, `!=`). This simplification is based on the dynamic typing nature of the Quantum language.

## Parameters

- None explicitly mentioned in the provided code snippet. However, it is implied that this function interacts with the broader parsing context, which includes:
  - `current()`: Retrieves the current token being processed.
  - `consume()`: Consumes the current token and advances to the next one.
  - `check(TokenType type)`: Checks if the current token matches the specified token type without consuming it.

## Return Value

- The function returns a unique pointer to an `ASTNode` representing the parsed equality expression. If no equality operators are found, it simply returns the result of `parseComparison()`.

## How It Works

1. **Initial Parsing**: The function starts by calling `parseComparison()` to parse the left-hand side of the equality expression. This initial call sets the variable `left` to the result of the comparison.

2. **Loop Through Equality Operators**: 
   - The function then enters a loop that continues as long as the current token is either `TokenType::EQ` (loose equality), `TokenType::NEQ` (loose inequality), `TokenType::STRICT_EQ` (strict equality), or `TokenType::STRICT_NEQ` (strict inequality).
   
3. **Token Consumption and Operator Handling**:
   - Inside the loop, the function consumes the current token using `consume()`. This token represents the equality operator (`==`, `!=`, `===`, `!==`).
   - Depending on the type of the consumed token, the function constructs a string representation of the operator. For strict equality (`===`), it uses `"=="`; for strict inequality (`!==`), it uses `"!="`. For loose equality (`==`) and loose inequality (`!=`), it directly uses the token's value.
   
4. **Right-Hand Side Parsing**: After handling the operator, the function calls `parseComparison()` again to parse the right-hand side of the equality expression. This result is stored in the variable `right`.
   
5. **Building AST Node**:
   - The function then creates a new `ASTNode` containing a `BinaryExpr` object. This `BinaryExpr` encapsulates the operator and the two operands (`left` and `right`).
   - The line number (`ln`) associated with the current token is passed to the `ASTNode` constructor to maintain accurate source location information.
   - The constructed `BinaryExpr` node becomes the new `left` operand for the next iteration of the loop, allowing for chained equality expressions like `a == b == c`.

6. **Exit Loop**: When no more equality operators are found, the loop exits, and the final `left` operand is returned, completing the parsing of the entire equality expression.

## Edge Cases

- **No Equality Operators**: If the input stream contains no equality operators after the initial comparison, the function will simply return the result of `parseComparison()`, effectively skipping the equality part of the expression.
- **Nested Expressions**: The function can handle nested equality expressions, such as `a == b && c == d`, by recursively calling itself within the loop.

## Interactions With Other Components

- **Parsing Context**: The function operates within the broader parsing context, utilizing helper functions like `current()`, `consume()`, and `check()` to manage the token stream.
- **Abstract Syntax Tree (AST)**: The function builds an AST by creating `BinaryExpr` nodes. These nodes represent the structure of the parsed expressions, facilitating further processing during compilation.
- **Dynamic Typing**: By treating strict equality as its non-strict counterpart, the function aligns with the quantum language's dynamic typing semantics, ensuring that the same rules apply regardless of whether the comparison is strict or not.