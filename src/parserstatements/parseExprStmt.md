# `parseExprStmt` Function

## Overview

The `parseExprStmt` function is a critical component of the Quantum Language compiler's parser module, responsible for parsing expressions that may be followed by semicolons or newlines. This function constructs an Abstract Syntax Tree (AST) node representing an expression statement.

### Why It Works This Way

This implementation allows for the parsing of comma-separated expressions within a single statement. For example, in the code `"a += 3, b = a"`, both `a += 3` and `b = a` are executed sequentially. The function handles these cases by checking for commas and constructing a `BlockStmt` to encapsulate each expression statement.

## Parameters/Return Value

- **Parameters**:
  - None explicitly defined in the provided code snippet.

- **Return Value**:
  - Returns a unique pointer to an `ASTNode` containing an `ExprStmt`.
  - If a comma-separated list of expressions is encountered, it returns a `BlockStmt` containing all the parsed `ExprStmt`s.

## Edge Cases

1. **Single Expression**: When only one expression is present, the function simply parses the expression and returns an `ExprStmt` wrapped in an `ASTNode`.

2. **Multiple Expressions Separated by Commas**: The function correctly identifies and parses multiple expressions separated by commas, grouping them into a `BlockStmt`. Each expression is evaluated in sequence.

3. **Semicolons and Newlines**: The function consumes any trailing semicolons or newlines after the expression, ensuring proper syntax handling.

4. **End of Input**: If the end of the input is reached before encountering a newline or semicolon, the function still correctly parses the remaining expressions.

## Interactions With Other Components

- **Tokenizer (`current()`, `consume()`)**: The function uses the tokenizer to fetch the current token and advance through the tokens when necessary. This interaction ensures that the parser can accurately identify and process different types of tokens, including expressions, commas, semicolons, and newlines.

- **Expression Parser (`parseExpr()`)**: The function calls `parseExpr()` to parse individual expressions. This interaction is essential for breaking down complex statements into their constituent parts.

- **Block Statement Construction (`BlockStmt`)**: When multiple expressions are found, they are grouped into a `BlockStmt`. This interaction demonstrates how the parser constructs more complex AST structures from simpler ones.

By understanding these aspects, developers can better grasp how the `parseExprStmt` function contributes to the overall parsing process of the Quantum Language compiler.