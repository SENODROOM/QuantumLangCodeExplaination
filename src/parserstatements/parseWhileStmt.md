# `parseWhileStmt`

The `parseWhileStmt` function is integral to the Quantum Language compiler's parser, tasked with processing while statements encountered in the source code. This function is essential for converting while loops into an Abstract Syntax Tree (AST) that can be further compiled into executable quantum code.

## What It Does

The primary responsibility of `parseWhileStmt` is to parse a while statement and construct the corresponding AST node. A while statement consists of a condition expression followed by a block of code that will execute repeatedly as long as the condition remains true.

## Why It Works This Way

1. **Condition Parsing**: The function begins by calling `parseExpr()` to extract the condition expression from the source code. This ensures that any valid expression can be used as the condition for the loop.

2. **Optional Colon Handling**: The next step involves matching an optional colon (`:`) token using `match(TokenType::COLON)`. This is a feature inspired by Python syntax, where a colon is often used after control flow statements like `while`.

3. **Skipping Newlines**: After optionally handling the colon, the function skips any newlines using `skipNewlines()`. This allows the parser to handle different styles of indentation or spacing without disrupting the parsing process.

4. **Body Parsing**: The function then parses the body of the while loop by calling `parseBodyOrStatement()`. This method is designed to handle both single statements and blocks of code, ensuring flexibility in how the loop body is structured.

5. **Creating AST Node**: Finally, the function constructs an `ASTNode` representing the while statement. This node contains two main elements:
   - The parsed condition expression.
   - The parsed body of the loop.

6. **Returning the AST Node**: The constructed `ASTNode` is returned, encapsulating the entire while statement structure within the AST.

## Parameters/Return Value

- **Parameters**:
  - None explicitly mentioned in the provided code snippet.

- **Return Value**:
  - Returns a unique pointer to an `ASTNode` object containing the parsed while statement. The `WhileStmt` struct holds the condition expression and the body of the loop.

## Edge Cases

1. **Empty Body**: If the while loop has an empty body (i.e., no statements follow the colon), the `parseBodyOrStatement()` method should handle this case gracefully, returning an appropriate AST node for an empty block.

2. **Invalid Condition Expression**: If the condition expression cannot be parsed correctly, `parseExpr()` should raise an error indicating the invalid syntax.

3. **Unexpected Tokens**: If tokens other than those expected for a while statement are encountered during parsing, the function should handle these errors appropriately, possibly skipping them or raising exceptions based on the context.

## Interactions With Other Components

- **Lexer**: The `current()` function likely retrieves the current token from the lexer, which provides the necessary information for parsing the while statement.

- **Expression Parser**: The `parseExpr()` function interacts with the expression parser to extract and validate the condition expression.

- **Body Parser**: The `parseBodyOrStatement()` function collaborates with the body parser to handle the loop body, which could involve parsing multiple statements or a block of code.

- **Error Handling**: Throughout the parsing process, various functions may interact with the error handler component to report syntax errors or unexpected tokens.

By understanding and implementing the `parseWhileStmt` function correctly, the Quantum Language compiler can effectively parse while statements and generate accurate AST nodes, facilitating subsequent compilation steps.