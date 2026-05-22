# parseReturnStmt

## Purpose
The `parseReturnStmt` function is designed to parse a return statement within the Quantum Language compiler's parser. Its primary role is to construct an abstract syntax tree (AST) node that accurately represents the return statement and any associated expressions. This ensures that the return statement can be correctly interpreted and executed during the compilation process.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: An `ASTNodePtr` representing the parsed return statement. If there is no expression following the `return` keyword, it returns an empty `ReturnStmt`.

## How It Works
1. **Initialization**: The function starts by recording the current line number (`ln`) using `current().line`.
2. **Expression Parsing**:
   - The function checks if the next token is not a newline, semicolon, or end of file. If so, it attempts to parse an expression using `parseExpr()`. This expression could be a single value or a tuple of values.
3. **Tuple Return Handling**:
   - If the next token after parsing the initial expression is a comma (`TokenType::COMMA`), indicating a tuple return, the function enters a loop to parse additional expressions until a comma is no longer followed by another valid expression.
   - Each parsed expression is added to a `TupleLiteral` object, which is then moved into an `ASTNode` containing a `ReturnStmt`.
4. **Whitespace Consumption**:
   - After parsing the main expression(s), the function consumes any trailing newlines or semicolons using a loop that checks for these tokens.
5. **Final Node Creation**:
   - The function creates and returns an `ASTNode` containing a `ReturnStmt`, passing in the parsed expression(s) as its argument. If no expression was found, it passes an empty `ReturnStmt`.

## Edge Cases
- **Empty Return Statement**: If the return statement does not follow the `return` keyword with any expression, the function will return an empty `ReturnStmt`.
- **Trailing Whitespace**: The function handles multiple trailing newlines or semicolons gracefully by consuming them all.

## Interactions With Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: While not explicitly shown in the code snippet, the function likely interacts with error handling mechanisms to report issues such as missing expressions or unexpected tokens.
- **Abstract Syntax Tree (AST)**: The function constructs nodes in the AST based on the parsed return statement, facilitating further processing during compilation.

This function is crucial for ensuring that return statements are correctly parsed and represented in the AST, enabling accurate execution of quantum programs during compilation.