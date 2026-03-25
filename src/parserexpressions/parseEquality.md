# `parseEquality` Function

## Purpose

The `parseEquality` function in the Quantum Language Compiler is designed to parse equality expressions from the source code. It handles both loose (`==`, `!=`) and strict (`===`, `!==`) equality comparisons.

## Parameters

- **None**: This function operates internally within the parser's state and does not take any explicit parameters.

## Return Value

- **`std::unique_ptr<ASTNode>`**: The function returns an abstract syntax tree (AST) node representing the parsed equality expression. If there are no equality operators present, it simply returns the result of parsing a comparison expression.

## How It Works

1. **Initial Parsing**:
   - The function starts by calling `parseComparison()` to get the left-hand side (LHS) of the equality expression. This LHS can be any valid comparison expression.

2. **Loop for Equality Operators**:
   - The function then enters a loop that continues as long as one of the following tokens is found at the current position: `TokenType::EQ` (loose equality), `TokenType::NEQ` (loose inequality), `TokenType::STRICT_EQ` (strict equality), or `TokenType::STRICT_NEQ` (strict inequality).

3. **Token Consumption and Conversion**:
   - Inside the loop, the function consumes the current token using `consume()`. It captures the line number (`ln`) where the operator occurs.
   - Depending on the type of the consumed token, it converts the token into a string representation of the operator. For example:
     - `TokenType::STRICT_EQ` is converted to `"=="`.
     - `TokenType::STRICT_NEQ` is converted to `"!="`.
     - Other tokens remain unchanged.

4. **Right-Hand Side Parsing**:
   - After consuming the operator, the function calls `parseComparison()` again to get the right-hand side (RHS) of the equality expression.

5. **Creating AST Node**:
   - The function creates a new `ASTNode` containing a `BinaryExpr` object. This `BinaryExpr` represents the binary operation between the LHS and RHS with the specified operator. The `BinaryExpr` constructor takes three arguments:
     - The operator as a string (`opStr`).
     - The LHS as a `std::unique_ptr<ASTNode>` (`std::move(left)`).
     - The RHS as a `std::unique_ptr<ASTNode>` (`std::move(right)`).
   - The line number (`ln`) is also passed to the `ASTNode` constructor to maintain context information.

6. **Updating Left-Hand Side**:
   - The newly created `ASTNode` becomes the new LHS for the next iteration of the loop, allowing for nested equality comparisons.

7. **Exit Loop**:
   - When no more equality operators are found, the loop exits, and the final `left` AST node is returned.

## Edge Cases

- **No Equality Operators**: If the input does not contain any equality operators (`==`, `!=`, `===`, `!==`), the function will simply return the result of parsing a comparison expression.
- **Nested Comparisons**: The function supports nested equality comparisons, meaning you can have expressions like `(a == b) == c`.

## Interactions with Other Components

- **Parsing Context**: The function relies on the parser's current state, including the ability to consume tokens (`consume()`) and check for specific token types (`check(TokenType::...)`).
- **Abstract Syntax Tree (AST)**: The function constructs an AST by creating `ASTNode` objects and linking them together with `BinaryExpr`. This AST is used for further semantic analysis and code generation.
- **Error Handling**: While not explicitly shown in the provided code snippet, the parser likely has mechanisms in place to handle errors such as missing operands or unexpected tokens during the parsing process.

This function is crucial for handling equality checks in the Quantum Language, ensuring that the grammar rules for these operations are correctly implemented and that the resulting AST accurately reflects the structure of the source code.