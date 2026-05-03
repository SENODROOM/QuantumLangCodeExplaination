# parseReturnStmt

## Purpose
The `parseReturnStmt` function is designed to parse a return statement within the Quantum Language compiler's parser. This function generates an abstract syntax tree (AST) node that encapsulates the return statement along with any associated expressions. The primary goal of this function is to ensure correct syntax and structure during compilation, facilitating subsequent semantic analysis and code generation phases.

## Parameters
- **None explicitly**: The function does not take any parameters directly. Instead, it relies on global state managed by the parser, such as the current token being processed (`current()`), which provides information about the next token in the input stream.

## Return Value
The function returns a unique pointer to an ASTNode object. This node represents the parsed return statement and contains either a single expression or a tuple literal (if multiple expressions are returned). The line number where the return statement begins is also stored in the ASTNode to maintain context throughout the compilation process.

## Implementation Details
1. **Initialization**: The function starts by recording the current line number (`ln`) using `current().line`. This ensures that the line number is correctly associated with the return statement in the AST.

2. **Expression Parsing**:
   - If the next token is not a newline, semicolon, or end of file, the function proceeds to parse an expression using `parseExpr()`. This expression can represent the value being returned.
   - After parsing the initial expression, the function checks if the next token is a comma (`TokenType::COMMA`). If so, it indicates a tuple return statement.

3. **Tuple Handling**:
   - A `TupleLiteral` object is created to hold multiple expressions.
   - The first expression is moved into the `elements` vector of the `TupleLiteral`.
   - The function then enters a loop, repeatedly checking for commas and parsing additional expressions until a newline, semicolon, or end of file is encountered.

4. **Consuming Delimiters**:
   - Once the main return statement or tuple has been parsed, the function consumes any remaining newlines or semicolons using a while loop. This ensures that the parser moves forward to the next valid token after encountering a return statement.

5. **Creating ASTNode**:
   - Finally, the function creates a unique pointer to an ASTNode containing a `ReturnStmt` object. If a tuple was parsed, the tuple literal is passed to the `ReturnStmt`; otherwise, the single expression is used.
   - The line number recorded earlier is passed to the `ReturnStmt` to maintain accurate source location information in the AST.

## Edge Cases
- **Single Expression**: When only one expression is returned, the function simply parses that expression and wraps it in a `ReturnStmt` without creating a tuple.
- **Multiple Expressions**: For tuple returns, the function continues to parse expressions until a delimiter (newline or semicolon) is encountered, ensuring that all elements of the tuple are correctly identified.
- **Empty Return Statement**: If the return statement does not include an expression, the function will still create a `ReturnStmt` node but with a null value. This allows for proper handling of empty return statements in the AST.

## Interactions with Other Components
- **Tokenizer**: The function uses the tokenizer to retrieve the current token and advance through the input stream.
- **Error Handling**: While not shown in the provided code snippet, the parser typically includes error handling mechanisms that would be invoked if invalid syntax were detected during the parsing of a return statement.
- **Semantic Analysis**: The generated AST nodes are crucial for semantic analysis, where the compiler checks the types and validity of expressions before generating machine code.
- **Code Generation**: During the code generation phase, the AST nodes produced by `parseReturnStmt` are used to emit the appropriate instructions for returning values from functions in the target quantum computing platform.

Overall, the `parseReturnStmt` function plays a vital role in accurately parsing return statements in the Quantum Language, contributing to the robustness and correctness of the compiler's output.