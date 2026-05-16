# parseReturnStmt

## Purpose
The `parseReturnStmt` function is designed to parse a return statement within the Quantum Language compiler's parser. Its primary role is to construct an abstract syntax tree (AST) node that accurately represents the return statement and any associated expressions. This ensures that the compiler can correctly interpret and process the return statement during compilation.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: An `ASTNodePtr` object representing the parsed return statement. If there is no expression following the `return` keyword, the returned node will contain a `nullptr`.

## How It Works
1. **Initialization**: The function begins by storing the current line number (`ln`) using `current().line`.
2. **Expression Parsing**: 
   - If the next token is not a newline, semicolon, or end of file, the function attempts to parse an expression using `parseExpr()`. This expression could be a single value or part of a tuple.
3. **Tuple Handling**:
   - If the next token is a comma (`TokenType::COMMA`), indicating a tuple return, the function enters a loop to parse additional elements of the tuple.
   - Each element is added to a `TupleLiteral` object, which is then moved into an `ASTNode` object.
4. **Whitespace Consumption**:
   - After parsing the expression(s), the function consumes any trailing newlines or semicolons until it reaches the end of the statement.
5. **Return Statement Construction**:
   - Finally, the function creates an `ASTNode` containing a `ReturnStmt` object, which holds the parsed expression(s). This node is returned as the result of the function.

## Edge Cases
- **Empty Return Statement**: If the return statement does not include an expression (e.g., `return;`), the function returns a node with a `nullptr` for the expression.
- **Single Expression**: If the return statement contains only one expression (e.g., `return x;`), the function parses and returns that expression directly.
- **Tuple Expressions**: If the return statement includes multiple expressions separated by commas (e.g., `return a, b, c;`), the function correctly handles them as a tuple literal.
- **Trailing Whitespace**: The function ignores any trailing newlines or semicolons after the return expression, ensuring that the AST remains clean and consistent.

## Interactions with Other Components
- **Tokenizer**: The function uses tokens provided by the tokenizer to determine the structure of the return statement.
- **Error Handling**: While not explicitly shown in the code snippet, the function likely interacts with error handling mechanisms to report issues such as missing expressions or unexpected tokens.
- **Scope Management**: The function may interact with scope management to ensure that variables referenced in the return statement are valid and accessible within their respective scopes.

By carefully parsing return statements and constructing appropriate AST nodes, `parseReturnStmt` plays a crucial role in the Quantum Language compiler's ability to accurately represent and compile quantum programs.