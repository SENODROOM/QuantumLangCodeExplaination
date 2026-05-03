# `parseExprStmt` Function

## Overview

The `parseExprStmt` function is a crucial component of the Quantum Language compiler's parser module, tasked with parsing expressions that may be followed by optional semicolons or newlines. It constructs an Abstract Syntax Tree (AST) node representing an expression statement, which is fundamental to the structure of any programming language.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: A unique pointer to an `ASTNode` object containing an `ExprStmt`. If the parsed expression is part of a comma-separated list, it returns a `BlockStmt` containing all the parsed statements.

## How It Works

1. **Initialization**:
   - The function begins by retrieving the current line number (`ln`) using `current().line`.
   - It then calls `parseExpr()` to parse the expression following the current token.

2. **Handling Comma Expressions**:
   - If the next token is a comma (`TokenType::COMMA`), indicating a comma-separated list of expressions, the function enters a loop to handle these expressions.
   - Inside the loop, it creates a `BlockStmt` to hold multiple statements. Each statement in the list is encapsulated within an `ExprStmt` and added to the `statements` vector of the `BlockStmt`.
   - The function continues to parse subsequent expressions until it encounters a newline (`TokenType::NEWLINE`), a semicolon (`TokenType::SEMICOLON`), or reaches the end of the input (`atEnd()`).

3. **Consuming Newlines and Semicolons**:
   - After parsing the main expression or handling the comma-separated list, the function consumes any remaining newlines or semicolons using `consume()`, ensuring proper syntax separation between statements.

4. **Edge Cases**:
   - If the parsed expression is not followed by any commas, newlines, or semicolons, the function simply returns an `ExprStmt` wrapped in an `ASTNode`.
   - When encountering a comma-separated list, the function ensures that each expression is executed sequentially, maintaining the order of operations as specified in the source code.

5. **Interactions with Other Components**:
   - `parseExpr()` is called to parse individual expressions, interacting with the lexer to retrieve tokens.
   - `skipNewlines()` is used to skip over consecutive newlines without adding them to the AST, allowing for cleaner syntax representation.
   - `consume()` is essential for advancing through the token stream after parsing expressions or handling syntax elements like newlines and semicolons.

In summary, the `parseExprStmt` function effectively handles both simple and complex expression statements, ensuring that the Quantum Language compiler can accurately parse and construct its AST based on the input source code. Its ability to manage comma-separated lists adds flexibility to the syntax, enabling more concise and expressive code.