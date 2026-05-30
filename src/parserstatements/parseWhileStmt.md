# `parseWhileStmt`

The `parseWhileStmt` function is a crucial component of the Quantum Language compiler's parser, responsible for converting while statements from the source code into an Abstract Syntax Tree (AST). This AST serves as the foundation for subsequent compilation phases.

## What It Does

The `parseWhileStmt` function processes a while statement in the source code and constructs an AST node representing that statement. Here’s a breakdown of its operations:

1. **Retrieve Line Number**: 
   ```cpp
   int ln = current().line;
   ```
   This line captures the current line number from the input stream, which is essential for error reporting and debugging purposes.

2. **Parse Condition**:
   ```cpp
   auto cond = parseExpr();
   ```
   The function calls `parseExpr()` to interpret the condition expression following the `while` keyword. This expression determines whether the loop should continue executing.

3. **Handle Optional Colon**:
   ```cpp
   match(TokenType::COLON); // optional Python-style colon
   ```
   This step checks for an optional colon (`:`) at the end of the while statement, similar to how Python handles its syntax. If the colon is present, it is consumed; otherwise, the function proceeds without any issues.

4. **Skip Newlines**:
   ```cpp
   skipNewlines();
   ```
   After handling the colon, the function skips any newline characters that might follow, ensuring that the parsing process continues on the correct line.

5. **Parse Body or Statement**:
   ```cpp
   auto body = parseBodyOrStatement();
   ```
   The function then parses the body of the while loop, which can be either a single statement or a block of statements enclosed within curly braces (`{}`). The result is stored in the `body` variable.

6. **Construct AST Node**:
   ```cpp
   return std::make_unique<ASTNode>(WhileStmt{std::move(cond), std::move(body)}, ln);
   ```
   Finally, the function creates a unique pointer to an `ASTNode` containing a `WhileStmt` object. This `WhileStmt` object encapsulates both the parsed condition and the body of the loop. The line number (`ln`) is also passed to provide context for potential errors or optimizations during the compilation process.

## Why It Works This Way

This design ensures that the while statement is accurately represented in the AST, allowing for proper semantic analysis and optimization during later stages of the compilation process. By separating the parsing of the condition and the body, the function maintains clarity and modularity, making it easier to manage and extend.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet.

- **Return Value**:
  - Returns a `std::unique_ptr<ASTNode>` containing an `ASTNode` with a `WhileStmt` object. The `WhileStmt` object holds the parsed condition and body of the while loop, along with the line number where the statement was found.

## Edge Cases

- **Empty Loop Body**: If the loop body consists solely of a semicolon (`;`), the `parseBodyOrStatement()` function will handle it appropriately, returning an empty block or a single statement node.
  
- **Missing Colon**: The function gracefully handles the absence of a colon at the end of the while statement, assuming the next token starts the body directly.

## Interactions With Other Components

- **Lexer**: The `current()` function likely retrieves the current token from the lexer, enabling the parser to determine the structure of the while statement.
  
- **Expression Parser**: The `parseExpr()` function is called to parse the condition expression, interacting with various lexical tokens to construct a valid expression tree.
  
- **Body Parser**: The `parseBodyOrStatement()` function is used to parse the body of the while loop, which could involve calling other parsing functions depending on the complexity of the body.

By integrating these components seamlessly, the `parseWhileStmt` function contributes to the overall robustness and functionality of the Quantum Language compiler's parser.