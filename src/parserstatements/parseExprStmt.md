# `parseExprStmt` Function

## Overview

The `parseExprStmt` function is a critical component of the Quantum Language compiler's parser module, responsible for parsing expressions that may be followed by semicolons or newlines. This function constructs an Abstract Syntax Tree (AST) node representing an expression statement.

### Why It Works This Way

The function handles both single and multiple expression statements separated by commas. If a comma is encountered after the initial expression, it treats the entire sequence as a block of statements executed sequentially. This allows for complex operations like updating variables in one line, such as `a += 3, b = a`.

## Parameters/Return Value

- **Parameters**:
  - None

- **Return Value**:
  - A unique pointer to an ASTNode containing either an ExprStmt or a BlockStmt, depending on whether the parsed expression contains multiple comma-separated expressions.

## Edge Cases

1. **Single Expression**: When only a single expression is present, the function returns an ASTNode with an ExprStmt containing that expression.
2. **Multiple Expressions Separated by Commas**: The function correctly parses and groups multiple expressions into a single BlockStmt, each wrapped in an ExprStmt.
3. **Trailing Semicolons or Newlines**: The function consumes any trailing semicolons or newlines after the expression(s).

## Interactions With Other Components

- **Tokenizer**: The function relies on the tokenizer to provide tokens for parsing.
- **Error Handling**: The function includes error handling mechanisms to manage unexpected token sequences gracefully.
- **Block Parsing**: When encountering multiple comma-separated expressions, the function uses the `BlockStmt` class to group these expressions together.

This comprehensive approach ensures that the parser can handle various syntax structures within quantum language programs effectively.