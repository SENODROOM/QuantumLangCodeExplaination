# `parseExprStmt` Function

## Overview

The `parseExprStmt` function is a critical component of the Quantum Language compiler's parser module, responsible for parsing expressions that can optionally be followed by semicolons or newlines. This function constructs an Abstract Syntax Tree (AST) node representing an expression statement. The primary purpose of this function is to handle both single and multiple expressions in a comma-separated list within a single statement.

## Parameters

- None

## Return Value

- Returns a unique pointer to an ASTNode containing an ExprStmt object. If the parsed expression is part of a larger comma-separated list, it returns a unique pointer to a BlockStmt containing all the parsed expressions.

## Edge Cases

1. **Single Expression**: When the input consists of a single expression followed by a semicolon or newline, the function correctly parses the expression into an ExprStmt and returns it.
2. **Multiple Expressions**: In cases where multiple expressions are separated by commas, the function processes them sequentially, creating a BlockStmt to hold all the expressions. Each expression is wrapped in its own ExprStmt node.
3. **Empty Statement**: If the input is just a semicolon or newline without any preceding expression, the function skips these tokens and returns an empty ExprStmt node.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to retrieve the current token (`current()`) and advance through the tokens using methods like `consume()` and `skipNewlines()`.
- **Expression Parser**: Within the function, it calls `parseExpr()` to parse individual expressions. This method is assumed to be defined elsewhere in the parser module.
- **Block Construction**: If multiple expressions are found, they are grouped into a BlockStmt, which allows for sequential execution of these expressions within the same statement context.

## Detailed Explanation

### Parsing Single Expression

When `parseExprStmt` encounters a single expression, it proceeds as follows:

1. Retrieves the line number of the current token using `current().line`.
2. Calls `parseExpr()` to parse the expression and stores the result in the `expr` variable.
3. Checks for optional trailing semicolons or newlines using `check(TokenType::SEMICOLON)` or `check(TokenType::NEWLINE)`. If found, it consumes them using `consume()`.
4. Constructs an ASTNode containing an ExprStmt with the parsed expression and the line number, then returns it.

### Handling Comma-Separated Expressions

For inputs containing multiple expressions separated by commas, the function behaves differently:

1. Again retrieves the line number of the first expression.
2. Parses the first expression using `parseExpr()` and stores it in `expr`.
3. Checks if the next token is a comma (`TokenType::COMMA`). If so, it enters a loop:
   - Initializes a `BlockStmt` to hold the expressions.
   - Adds the initial expression wrapped in an ExprStmt to the block.
   - Continues to parse subsequent expressions until a non-comma token is encountered (semicolon, newline, or end of file).
   - For each parsed expression, it retrieves the line number, wraps it in an ExprStmt, and adds it to the block.
4. After exiting the loop, it continues to consume optional trailing semicolons or newlines.
5. Finally, it returns a unique pointer to the constructed `BlockStmt`.

This approach ensures that the parser can handle complex statements involving multiple operations in a single line, which is common in quantum programming languages due to their nature of performing multiple operations simultaneously.