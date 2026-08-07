# parseReturnStmt

## Purpose
The `parseReturnStmt` function is crucial for parsing return statements within the Quantum Language compiler's parser. Its primary role is to construct an abstract syntax tree (AST) node that accurately represents a return statement, including any associated expressions. This function ensures that the return statement adheres to the grammar rules specified in the language, allowing for both simple and complex return scenarios.

## Parameters/Return Value
- **Parameters**: None explicitly listed in the provided code snippet.
- **Return Value**: The function returns a unique pointer to an `ASTNode` object, which encapsulates a `ReturnStmt`. If there is a return value, it is wrapped inside a `TupleLiteral` if multiple values are returned, otherwise, it remains as a single expression. The `ReturnStmt` contains the parsed return value and the line number where the return statement was encountered.

## How It Works
1. **Initialization**: The function begins by retrieving the current line number (`ln`) using the `current().line` method call.
2. **Parsing Return Value**: It then checks if the next token is not a newline, semicolon, or closing brace (`RBRACE`). If these conditions are met, it proceeds to parse the return value using the `parseExpr()` function. This allows for handling both simple and complex expressions that can be returned.
3. **Handling Tuple Returns**: If the next token after parsing the initial return value is a comma (`TokenType::COMMA`), indicating a tuple return, the function enters a loop to parse subsequent expressions until a non-comma token is encountered. Each parsed expression is added to a `TupleLiteral`, which is then moved into the final `ReturnStmt`.
4. **Consuming Whitespace**: After parsing the return value(s), the function consumes any remaining newlines or semicolons using a while loop. This ensures that the parser moves past any trailing whitespace or punctuation without prematurely ending the statement.
5. **Creating AST Node**: Finally, the function creates a unique pointer to an `ASTNode` containing a `ReturnStmt`. If a return value was parsed, it is included; otherwise, the `ReturnStmt` is empty. The line number is passed along to ensure accurate error reporting and debugging.

## Edge Cases
- **Empty Return Statement**: If the return statement does not include any value (e.g., `return;`), the function correctly handles this by returning an empty `ReturnStmt`.
- **Single Expression Return**: For a return statement with a single expression (e.g., `return x;`), the function parses the expression and wraps it directly in the `ReturnStmt`.
- **Multiple Expressions Return**: When dealing with a return statement that includes multiple expressions (e.g., `return a, b;`), the function properly parses each expression and groups them into a `TupleLiteral`.

## Interactions With Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing. It uses methods like `current()`, `check()`, and `match()` to interact with the tokenizer and determine the type of token being processed.
- **Error Handling**: While not explicitly shown in the provided code snippet, the function likely interacts with error handling mechanisms within the compiler to report errors related to invalid return statements or unexpected tokens.
- **Scope Management**: Although not covered here, the function may indirectly interact with scope management components when parsing expressions that reference variables or functions defined in different scopes.

In summary, the `parseReturnStmt` function is essential for correctly parsing return statements in the Quantum Language compiler. By carefully handling both simple and complex return scenarios, it ensures that the resulting AST accurately reflects the structure and intent of the source code.