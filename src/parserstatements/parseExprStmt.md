# `parseExprStmt` Function

## Overview

The `parseExprStmt` function is a crucial component of the Quantum Language compiler's parser module, responsible for parsing expressions that may be followed by semicolons or newlines. This function constructs an Abstract Syntax Tree (AST) node representing an expression statement. The primary purpose of this function is to handle both single and multiple expressions separated by commas, ensuring they are correctly parsed into an AST structure.

### Why It Works This Way

The function works in a way that allows it to handle both simple and complex expressions within a single statement. By checking for the presence of a comma token (`TokenType::COMMA`), it can determine whether the statement contains multiple expressions. If a comma is found, the function parses each expression sequentially and groups them into a `BlockStmt`, which represents a block of statements. This approach ensures that all expressions are evaluated in the correct order, mimicking the behavior of a C-style comma expression.

### Parameters/Return Value

- **Parameters**:
  - None explicitly listed; however, it relies on global state managed by the parser, such as the current token being processed.

- **Return Value**:
  - Returns a unique pointer to an `ASTNode` containing either a single `ExprStmt` or a `BlockStmt` depending on whether multiple expressions were encountered.

### Edge Cases

1. **Single Expression**: When the input consists of a single expression without any commas, the function simply returns an `ExprStmt` node wrapping the parsed expression.
2. **Multiple Expressions Separated by Commas**: If multiple expressions are separated by commas, the function correctly parses each expression and groups them into a `BlockStmt`. Each expression is wrapped in its own `ExprStmt`.
3. **Trailing Commas**: The function handles trailing commas gracefully by breaking out of the loop when encountering a newline, semicolon, or end of file.
4. **Empty Statements**: The function consumes any trailing semicolons or newlines after the expression, effectively handling empty statements.

### Interactions With Other Components

- **Tokenizer**: The function interacts with the tokenizer to retrieve the current token and move to the next one during parsing.
- **Error Handling**: While not explicitly shown in the code snippet, the function likely integrates with the error handling mechanism of the compiler to report syntax errors appropriately.
- **AST Construction**: The function constructs nodes in the AST based on the parsed expressions and their positions in the source code. These nodes include `ExprStmt` and potentially `BlockStmt`.

By understanding how `parseExprStmt` functions, developers can better grasp the overall structure and flow of the Quantum Language compiler's parsing process, particularly in handling complex expressions and managing the resulting AST.