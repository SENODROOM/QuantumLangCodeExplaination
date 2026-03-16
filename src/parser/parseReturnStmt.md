# `parseReturnStmt`

## Purpose

The `parseReturnStmt` function is designed to parse a return statement in the source code of the Quantum Language. It constructs an abstract syntax tree (AST) node that represents the return statement along with any expressions it may contain. This function ensures proper handling of both single and tuple return statements, as well as optional trailing newlines or semicolons.

## Parameters

- **None explicitly**: The function does not take any explicit parameters. Instead, it relies on global state such as the current token being processed and methods like `current()`, `check()`, and `match()` to determine the structure of the return statement.

## Return Value

The function returns a unique pointer to an `ASTNode` object, which encapsulates a `ReturnStmt`. The `ReturnStmt` contains a unique pointer to either a single `ASTNode` representing the returned expression or a `TupleLiteral` containing multiple expressions if a tuple return is detected.

## Edge Cases

1. **Single Expression Return**: If the return statement consists of a single expression followed by a newline or semicolon, the function correctly parses this into a `ReturnStmt` with a single `ASTNode`.
2. **Tuple Return**: If the return statement includes multiple comma-separated expressions, the function correctly identifies this as a tuple return and constructs a `TupleLiteral` containing all the parsed expressions.
3. **Trailing Newline/Semicolon**: The function consumes any trailing newlines or semicolons after the return statement, ensuring the parser can continue processing subsequent tokens without interruption.

## Interactions with Other Components

- **Tokenizer**: The function uses `current()` to access the current token in the input stream, relying on the tokenizer to provide the correct sequence of tokens for parsing.
- **Expression Parser**: When a single expression is encountered, the function calls `parseExpr()` to parse the expression into an `ASTNode`. This interaction allows the function to handle complex expressions within the return statement.
- **Error Handling**: Although not shown in the provided code snippet, the function likely interacts with error handling mechanisms to report issues such as missing expressions or unexpected tokens during parsing.
- **Abstract Syntax Tree Construction**: The function constructs an `ASTNode` that represents the return statement. This node is then used by higher-level components to build the complete AST of the program.

By carefully managing these interactions, `parseReturnStmt` ensures robust parsing of return statements in the Quantum Language, supporting both simple and complex return scenarios efficiently.