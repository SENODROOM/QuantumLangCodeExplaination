# `parseExprStmt` Function

## Overview

The `parseExprStmt` function is a crucial component of the Quantum Language compiler's parser module, responsible for parsing expressions that may be followed by semicolons or newlines. This function constructs an Abstract Syntax Tree (AST) node representing an expression statement.

### Why It Works This Way

This implementation allows for the parsing of complex expressions, including those separated by commas, which can be executed in sequence within a single statement. The function checks for commas and handles them accordingly, creating a `BlockStmt` to encapsulate each individual expression statement.

## Parameters/Return Value

- **Parameters**:
  - None

- **Return Value**:
  - A unique pointer to an `ASTNode` containing an `ExprStmt`. If the expression is part of a comma-separated list, it returns a `BlockStmt`.

## Edge Cases

1. **Single Expression**: When there is only one expression, the function simply creates an `ExprStmt` and returns it.
2. **Comma-Separated Expressions**: When multiple expressions are separated by commas, the function parses each expression into its own `ExprStmt`, encapsulates them in a `BlockStmt`, and returns the `BlockStmt`.
3. **Trailing Commas**: The function skips any trailing commas after the last expression.
4. **Semicolons and Newlines**: The function consumes any semicolons or newlines following the expression statement.

## Interactions With Other Components

- **Tokenizer**: The function uses the tokenizer to retrieve tokens (`current()`) and advance through the token stream (`consume()`).
- **Expression Parser**: The function calls `parseExpr()` to parse the expression itself.
- **Error Handling**: Although not explicitly shown in the code snippet, the function should interact with error handling mechanisms to manage syntax errors gracefully.

## Detailed Explanation

Here's a step-by-step breakdown of how the function operates:

1. **Retrieve Current Line Number**: The function starts by retrieving the line number of the current token using `current().line`.
2. **Parse Expression**: It then calls `parseExpr()` to parse the expression.
3. **Check for Comma Separators**: The function checks if the next token is a comma (`TokenType::COMMA`). If so, it enters a loop to handle multiple expressions separated by commas.
4. **Create Block Statement**: Inside the loop, it creates a `BlockStmt` to hold the parsed expression statements.
5. **Add Initial Expression Statement**: The initial expression is wrapped in an `ExprStmt` and added to the `statements` vector of the `BlockStmt`.
6. **Loop Through Comma-Separated Expressions**: The function continues to parse subsequent expressions until a non-comma token is encountered (such as a newline, semicolon, or end of file).
7. **Skip Trailing Commas**: After parsing all expressions, the function skips any trailing commas.
8. **Consume Semicolons and Newlines**: Finally, the function consumes any remaining semicolons or newlines before returning the constructed AST node.

This approach ensures that the parser can handle both simple and complex expression statements efficiently, providing flexibility in the syntax of the Quantum Language.