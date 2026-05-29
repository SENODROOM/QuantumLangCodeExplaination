# parseReturnStmt

## Purpose
The `parseReturnStmt` function is designed to parse a return statement within the Quantum Language compiler's parser. Its primary role is to construct an abstract syntax tree (AST) node that accurately represents the return statement and any associated expressions. This ensures that the return statement can be properly evaluated during compilation and execution.

## Parameters
- None

## Return Value
- An `ASTNodePtr` representing the parsed return statement. The returned node contains a `ReturnStmt` object which holds the expression to be returned.

## Edge Cases
1. **Empty Return Statement**: If the return statement does not contain any expression, the function will simply create a `ReturnStmt` node without any value.
2. **Single Expression**: If the return statement contains only one expression, the function will parse that expression and store it in the `ReturnStmt` node.
3. **Tuple Return**: If the return statement contains multiple expressions separated by commas (e.g., `return a, b;`), the function will parse each expression into elements of a tuple and store the tuple in the `ReturnStmt` node.

## How It Works
The function starts by recording the line number (`ln`) where the return statement begins. It then checks if the next token is either a newline, semicolon, or end of file. If none of these conditions are met, it proceeds to parse an expression using the `parseExpr()` method.

If the parsed expression is followed by a comma, indicating a tuple return, the function enters a loop to continue parsing additional expressions until a comma is encountered again or the end of the statement is reached. Each parsed expression is added as an element to a `TupleLiteral`.

After handling the expression(s), the function consumes any trailing newlines or semicolons to ensure proper parsing of subsequent statements. Finally, it constructs an `ASTNode` containing a `ReturnStmt` object with the parsed expression(s) and returns this node.

## Interactions With Other Components
- **Tokenizer**: The function relies on the tokenizer to provide the next token for parsing.
- **Abstract Syntax Tree (AST)**: The parsed return statement is represented as an `ASTNode` containing a `ReturnStmt` object, which is then used by the rest of the compiler to generate executable code or perform semantic analysis.
- **Error Handling**: While not explicitly shown in the provided code snippet, the function likely interacts with error handling mechanisms to report errors such as missing expressions after a comma or unexpected tokens in the return statement.