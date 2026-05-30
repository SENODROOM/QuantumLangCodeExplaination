# parseReturnStmt

## Purpose
The `parseReturnStmt` function is responsible for parsing a return statement in the Quantum Language compiler's parser. It constructs an abstract syntax tree (AST) node representing the return statement and any associated expressions.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: An `ASTNodePtr` pointing to the parsed return statement node.

## How It Works
1. **Initialization**: The function starts by recording the current line number (`ln`) where the return statement begins.
2. **Expression Parsing**:
   - If the next token is not a newline, semicolon, or end of file, the function attempts to parse an expression using `parseExpr()`. This expression will be the value returned by the function.
3. **Tuple Return Handling**:
   - If the next token after the expression is a comma (`TokenType::COMMA`), indicating a tuple return, the function enters a loop to handle multiple expressions separated by commas.
   - A `TupleLiteral` object is created to store the elements of the tuple.
   - The first element is added to the tuple using `tup.elements.push_back(std::move(val))`.
   - The loop continues to parse additional expressions until a comma followed by a newline, semicolon, or end of file is encountered.
   - Each parsed expression is added to the tuple.
4. **Semicolon or Newline Consumption**:
   - After parsing the expression(s), the function consumes any trailing semicolons or newlines using a while loop. This ensures that the parser can correctly handle statements terminated by either type of punctuation.
5. **Return Statement Construction**:
   - Finally, the function returns a unique pointer to an `ASTNode` containing a `ReturnStmt` object. The `ReturnStmt` object holds the parsed expression(s) as its value.

## Edge Cases
- **No Expression**: If the return statement does not include an expression (e.g., `return;`), the function will still create a `ReturnStmt` node with a null value.
- **Single Expression**: If the return statement includes only one expression, the function will parse that single expression and wrap it in a `ReturnStmt` node.
- **Multiple Expressions**: If the return statement includes multiple expressions separated by commas, the function will parse each expression and add them to a `TupleLiteral`, which is then wrapped in a `ReturnStmt` node.
- **Trailing Punctuation**: The function handles both semicolons and newlines following the return statement, ensuring proper parsing even when these tokens are present.

## Interactions With Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the next token for parsing.
- **Error Handling**: While not explicitly shown in the code snippet, error handling mechanisms would typically interact with this function to manage unexpected tokens or syntax errors during parsing.
- **Abstract Syntax Tree (AST)**: The parsed return statement and associated expressions are constructed as nodes in the AST, facilitating further compilation steps such as semantic analysis and code generation.