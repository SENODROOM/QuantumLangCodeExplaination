# parseReturnStmt

## Purpose
The `parseReturnStmt` function is responsible for parsing a return statement in the Quantum Language compiler's parser. It constructs an abstract syntax tree (AST) node representing the return statement and any associated expressions. The main goal is to ensure that the return statement is correctly parsed and that the resulting AST accurately reflects the structure of the code.

## Parameters
- None

## Return Value
- An `ASTNodePtr` object pointing to the newly created AST node representing the return statement.

## How It Works
1. **Initialization**: The function starts by recording the current line number (`ln`) using the `current().line` method.
2. **Expression Parsing**: If the next token is not a newline, semicolon, or end of file, the function attempts to parse an expression using the `parseExpr()` method. This expression represents the value being returned.
3. **Tuple Handling**: If the next token after the expression is a comma, indicating a tuple return, the function enters a loop to handle multiple elements:
   - A `TupleLiteral` object is initialized to store the elements of the tuple.
   - The first element of the tuple is added to the `tup.elements` vector.
   - The function then continues to parse additional expressions separated by commas until it encounters a newline, semicolon, or reaches the end of the file.
4. **Whitespace Consumption**: After parsing the return value(s), the function consumes any trailing newlines or semicolons using a loop that checks for these tokens and calls the `consume()` method.
5. **Return Statement Node Creation**: Finally, the function creates an `ASTNode` object containing a `ReturnStmt` with the parsed value(s) and returns it.

## Edge Cases
- **Empty Return Statement**: If there is no expression following the `return` keyword, the function will simply create a `ReturnStmt` without a value.
- **Single Expression Return**: If only one expression follows the `return` keyword, it is treated as a single-element tuple.
- **Multiple Expressions Return**: If multiple expressions follow the `return` keyword, they are collected into a tuple.
- **Trailing Whitespace**: Any trailing newlines or semicolons after the return statement are consumed to maintain consistent parsing behavior.

## Interactions With Other Components
- **Tokenizer**: The `current()` and `consume()` methods rely on the tokenizer to provide the next token and advance through the input stream, respectively.
- **Error Handling**: While not explicitly shown in the provided code snippet, the tokenizer and parser should be equipped with error handling mechanisms to manage unexpected tokens or malformed statements gracefully.
- **Abstract Syntax Tree (AST)**: The function interacts with the AST by creating nodes for the return statement and its associated values. These nodes are used throughout the compilation process to represent the syntactic structure of the code.

This function ensures that return statements are correctly parsed and represented in the AST, facilitating further processing during the compilation phase.