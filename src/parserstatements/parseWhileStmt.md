# `parseWhileStmt`

The `parseWhileStmt` function is a critical component of the Quantum Language compiler's parser, responsible for interpreting and converting while statements found within the source code into an Abstract Syntax Tree (AST). This AST serves as a foundational structure for subsequent compilation phases.

## What It Does

The `parseWhileStmt` function processes a while statement in the source code and constructs an AST node representing that statement. The while statement typically consists of a condition followed by a block of code that will be executed repeatedly as long as the condition remains true.

## Why It Works This Way

1. **Condition Parsing**: The function starts by parsing the condition expression using `parseExpr()`. This ensures that the condition is correctly interpreted and represented in the AST.
2. **Optional Colon Handling**: The function then checks for an optional Python-style colon (`:`) using `match(TokenType::COLON)`. This allows for flexibility in the syntax of the while statement.
3. **Skipping Newlines**: After handling the colon, the function skips any newlines using `skipNewlines()`. This ensures that the parser can continue processing the next part of the statement without being affected by whitespace.
4. **Body Parsing**: The function parses the body of the while loop using `parseBodyOrStatement()`. This could either be a single statement or a block of statements enclosed in curly braces `{}`.
5. **AST Node Creation**: Finally, the function creates an AST node for the while statement using `std::make_unique<ASTNode>(WhileStmt{std::move(cond), std::move(body)}, ln)`, where `ln` is the line number of the current token. The `WhileStmt` struct contains the parsed condition and body.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet, but implicitly expects tokens to be available via some global state or context managed by the parser.
  
- **Return Value**:
  - Returns a unique pointer to an `ASTNode` object representing the parsed while statement. The `ASTNode` encapsulates a `WhileStmt` which holds the parsed condition and body.

## Edge Cases

1. **Missing Condition**: If the condition expression is missing, the function should handle this gracefully, possibly throwing an error or producing a default condition.
2. **Empty Body**: If the body of the while loop is empty, the function should still construct a valid AST node, indicating that the loop has no actions to perform.
3. **Syntax Errors**: The function should robustly handle syntax errors such as unexpected tokens or misplaced colons.

## Interactions With Other Components

- **Token Matching**: The function interacts with the lexer to match specific token types like `TokenType::COLON`.
- **Expression Parsing**: It uses `parseExpr()` to interpret the condition expression, which may involve multiple sub-parsers depending on the complexity of the expression.
- **Body Parsing**: The function calls `parseBodyOrStatement()` to process the body of the while loop, which could involve further nested parsing.
- **Error Handling**: The function might interact with an error reporting mechanism to log issues encountered during parsing.

This comprehensive approach ensures that the while statement is accurately represented in the AST, facilitating correct compilation and execution of quantum programs.