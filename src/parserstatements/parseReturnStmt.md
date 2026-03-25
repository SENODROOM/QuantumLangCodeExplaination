# parseReturnStmt

## Purpose
The `parseReturnStmt` function is responsible for parsing a return statement in the Quantum Language compiler's parser. It constructs an abstract syntax tree (AST) node representing the return statement and its associated expression(s).

## Parameters
- None explicitly listed as parameters, but it relies on global state managed by the parser:
  - `current()`: Returns the current token being processed.
  - `check(TokenType type)`: Checks if the next token matches the specified token type without consuming it.
  - `match(TokenType type)`: Consumes the next token if it matches the specified token type.
  - `atEnd()`: Determines if all tokens have been processed.

## Return Value
The function returns a unique pointer to an `ASTNode` object, which encapsulates a `ReturnStmt`. The `ReturnStmt` contains a unique pointer to an `ASTNode` representing the returned value. If there is no returned value, the `ReturnStmt` will contain a null pointer.

## Edge Cases
1. **No Expression**: If the return statement does not follow by any expression (i.e., it ends with a semicolon or newline), the function correctly handles this by returning a `ReturnStmt` with a null pointer.
2. **Single Expression**: If the return statement follows by a single expression, the function parses that expression and wraps it in a `ReturnStmt`.
3. **Tuple Expression**: If the return statement follows by multiple expressions separated by commas, the function parses these expressions into a tuple and wraps the tuple in a `ReturnStmt`.

## Interaction with Other Components
- **Tokenizer**: The function uses the tokenizer to retrieve the current token and subsequent tokens.
- **Expression Parser (`parseExpr()`)**: When encountering a valid expression after the `return` keyword, the function calls `parseExpr()` to parse the expression into an `ASTNode`.
- **Abstract Syntax Tree (`ASTNode`)**: The parsed expression is wrapped in an `ASTNode`, which is then used to construct a `ReturnStmt`.
- **Error Handling**: Although not shown in the snippet, the function likely interacts with error handling mechanisms of the parser to manage unexpected token sequences or missing expressions.

## Detailed Explanation
1. **Initialization**:
   ```cpp
   int ln = current().line;
   ```
   This line retrieves the line number where the return statement starts.

2. **Parsing the Returned Value**:
   ```cpp
   ASTNodePtr val;
   if (!check(TokenType::NEWLINE) && !check(TokenType::SEMICOLON) && !atEnd())
   {
       val = parseExpr();
   ```
   The function checks if the next token is not a newline, semicolon, or end of file. If so, it proceeds to parse an expression using `parseExpr()` and assigns it to `val`.

3. **Handling Tuple Return Statements**:
   ```cpp
   if (check(TokenType::COMMA))
   {
       TupleLiteral tup;
       tup.elements.push_back(std::move(val));
       while (match(TokenType::COMMA))
       {
           if (check(TokenType::NEWLINE) || check(TokenType::SEMICOLON) || atEnd())
               break;
           tup.elements.push_back(parseExpr());
       }
       val = std::make_unique<ASTNode>(std::move(tup), ln);
   }
   ```
   If the next token is a comma, indicating a tuple return, the function enters a loop to parse each element of the tuple. Each parsed expression is added to the `tup.elements` vector. The loop continues until a comma is no longer followed by another expression, or the end of the statement is reached.

4. **Consuming Newlines and Semicolons**:
   ```cpp
   while (check(TokenType::NEWLINE) || check(TokenType::SEMICOLON))
       consume();
   ```
   After parsing the returned value, the function consumes any trailing newlines or semicolons to ensure proper parsing of subsequent statements.

5. **Constructing the Return Statement Node**:
   ```cpp
   return std::make_unique<ASTNode>(ReturnStmt{std::move(val)}, ln);
   ```
   Finally, the function creates a `ReturnStmt` containing the parsed value and the original line number, and wraps it in an `ASTNode` before returning it.

This function ensures that return statements are correctly parsed and represented in the AST, supporting both simple and complex return values including tuples.