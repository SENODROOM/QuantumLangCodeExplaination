# parseReturnStmt

## Purpose
The `parseReturnStmt` function is designed to parse a return statement within the Quantum Language compiler's parser. Its primary role is to construct an abstract syntax tree (AST) node that accurately represents the return statement and any associated expressions. This ensures that the return statement can be correctly interpreted and executed during compilation.

## Parameters
- None

## Return Value
- An `ASTNodePtr` representing the parsed return statement. If there is no expression following the `return` keyword, it returns an empty `ReturnStmt`.

## How It Works
1. **Initialization**: The function begins by storing the current line number (`ln`) where the return statement starts.
2. **Expression Parsing**: It checks if the next token is not a newline, semicolon, or end of file. If so, it parses an expression using the `parseExpr()` method and assigns it to `val`.
3. **Tuple Handling**: If the next token after parsing the expression is a comma, indicating a tuple return, it enters a loop to parse additional expressions until a comma is no longer found or the end of the statement is reached. Each parsed expression is added to a `TupleLiteral` object.
4. **Completion**: After parsing all expressions, including those in a tuple, it consumes any trailing newlines or semicolons using a while loop.
5. **Return Statement Construction**: Finally, it constructs an `ASTNode` containing a `ReturnStmt` object, which holds the parsed expression(s). If no expression was parsed, it contains an empty `ReturnStmt`. The constructed `ASTNode` is then returned.

## Edge Cases
- **Empty Return Statement**: If the return statement does not have an associated expression, the function will simply return an empty `ReturnStmt`.
- **Trailing Newlines/Semicolons**: Any newlines or semicolons that follow the return statement are consumed to ensure proper parsing of subsequent statements.
- **Tuple Returns**: Multiple values can be returned as a tuple, separated by commas. The function handles this by constructing a `TupleLiteral` with multiple elements.

## Interactions With Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: While not explicitly shown in the code snippet, the function should interact with error handling mechanisms to manage unexpected tokens or malformed return statements gracefully.
- **Abstract Syntax Tree (AST)**: The parsed return statement is integrated into the AST, allowing further analysis and transformation during the compilation process.