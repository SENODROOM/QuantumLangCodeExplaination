# `parseBodyOrStatement` Function

## Overview
The `parseBodyOrStatement` function determines whether the next sequence of tokens in the source code represents either a statement body or a standalone statement. This function is crucial within the Quantum Language compiler as it helps in parsing control flow structures like loops and conditionals correctly.

### Parameters
- None

### Return Value
- Returns a unique pointer to an `ASTNode` object representing either a `BlockStmt` or a single `Statement`.

## Detailed Explanation
The function operates based on the type of token that follows immediately after the initial token being parsed. Here’s how it works:

1. **Check for Empty Body**:
   - If the next token is a semicolon (`TokenType::SEMICOLON`), it indicates an empty body for control flow statements such as `while` or `for`.
   - The function consumes the semicolon and returns a `BlockStmt` node wrapped in an `ASTNode`. This effectively means that the loop or conditional has no body, and its execution will be determined solely by its condition.

2. **Check for Block Start**:
   - If the next token is an opening brace (`TokenType::LBRACE`) or an indentation (`TokenType::INDENT`), it signifies the start of a block, which contains multiple statements.
   - The function calls `parseBlock()` to handle the parsing of the block. This method processes all tokens until it encounters a closing brace or unindentation, building up a list of statements within the block.

3. **Single Statement**:
   - If neither an empty body nor a block start is detected, the function assumes that the next sequence of tokens forms a single standalone statement.
   - It initializes a `BlockStmt` and pushes the result of calling `parseStatement()` into the `statements` vector of the `BlockStmt`.
   - Finally, it wraps the `BlockStmt` in an `ASTNode` and returns it.

## Edge Cases
- **Empty Body**: When encountering a semicolon directly after a control structure keyword, indicating an empty body.
- **Block Start**: Handling both opening braces and indentation to identify the beginning of a block containing multiple statements.
- **Single Statement**: Parsing scenarios where only one statement exists without any blocks or additional tokens.

## Interactions with Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the next token for analysis.
- **ASTNode**: Used to construct the abstract syntax tree (AST) nodes representing the parsed statements or blocks.
- **parseBlock()**: Invoked when a block start is detected, ensuring proper handling of multiple statements within a block.
- **parseStatement()**: Called when parsing a single standalone statement, ensuring correct statement construction.

This function is integral to the parser's ability to accurately interpret and construct the AST for Quantum Language programs, facilitating subsequent compilation steps.