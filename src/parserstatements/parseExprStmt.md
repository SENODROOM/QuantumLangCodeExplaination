# `parseExprStmt` Function

## Overview

The `parseExprStmt` function is part of the Quantum Language compiler's parser module and is responsible for parsing expressions followed by optional semicolons or newlines. This function constructs an AST node representing an expression statement.

## Parameters and Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet; however, it relies on global state managed by the parser class, such as `current()`, `check()`, and `match()` methods which operate on tokens.

- **Return Value**:
  - Returns a unique pointer to an `ASTNode` containing either an `ExprStmt` or a `BlockStmt`. If a comma-separated list of expressions is encountered, it returns a `BlockStmt`; otherwise, it returns an `ExprStmt`.

## How It Works

1. **Initialization**: The function starts by recording the current line number (`ln`) using `current().line`.
2. **Parsing Expression**: It then calls `parseExpr()` to parse the expression that follows the statement.
3. **Handling Comma Expressions**:
   - If a comma token (`TokenType::COMMA`) is found immediately after the parsed expression, it indicates the start of a comma-separated list of expressions.
   - A `BlockStmt` is initialized to hold multiple statements.
   - The first expression is wrapped in an `ExprStmt` and added to the `statements` vector of the `BlockStmt`.
   - The function enters a loop where it repeatedly checks for commas and parses additional expressions until a newline, semicolon, or end of file is encountered.
4. **Consuming Newlines and Semicolons**:
   - After processing any comma-separated expressions, the function consumes all subsequent newlines and semicolons using a while loop.
5. **Returning the Result**:
   - If a comma-separated list was processed, the function returns a unique pointer to the `BlockStmt`.
   - If no comma-separated list was found, the function returns a unique pointer to an `ExprStmt` containing the parsed expression.

## Edge Cases

- **Single Expression**: When only one expression is present without any trailing commas, the function correctly returns an `ExprStmt`.
- **Empty Statement**: An empty statement (followed by a semicolon or newline) results in a `nullptr` being returned, indicating the absence of a valid statement.
- **Comma Separated List**: When encountering a comma-separated list of expressions, the function handles them as multiple statements within a single block.
- **Trailing Commas**: Trailing commas at the end of a statement are ignored, and the function continues parsing the next token.

## Interactions with Other Components

- **Token Stream**: The function interacts with the token stream through methods like `current()`, `check()`, and `match()`. These methods allow the parser to look ahead at tokens and decide how to proceed with parsing.
- **AST Construction**: The function constructs nodes in the Abstract Syntax Tree (AST). Specifically, it creates `ExprStmt` nodes for individual expressions and `BlockStmt` nodes when handling comma-separated lists.
- **Error Handling**: While not shown in the provided code snippet, the parser likely includes error handling mechanisms to manage unexpected tokens or syntax errors gracefully.

This function is crucial for parsing statements in the Quantum Language, ensuring that both simple expressions and complex comma-separated lists are handled appropriately.