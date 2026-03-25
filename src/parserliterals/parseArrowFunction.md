# `parseArrowFunction`

## Purpose

The `parseArrowFunction` method is responsible for parsing arrow functions in the Quantum Language compiler. Arrow functions provide a concise syntax for defining anonymous functions and are commonly used for creating callbacks or simple operations.

## Parameters

- **None**: This method does not take any explicit parameters. It relies on the global state of the parser to access the current token and the list of parsed parameters (`params`).

## Return Value

- **`std::unique_ptr<ASTNode>`**: The method returns a unique pointer to an abstract syntax tree (AST) node representing the parsed arrow function. The AST node contains information about the function's parameters and body.

## Implementation Details

1. **Consume FAT_ARROW or ARROW Token**:
   ```cpp
   if (!match(TokenType::FAT_ARROW) && !match(TokenType::ARROW))
       throw ParseError("Expected '=>' or '->'", current().line, current().col);
   ```
   - The method first checks if the current token is either a `FAT_ARROW` (`=>`) or an `ARROW` (`->`). If neither token is found, it throws a `ParseError` indicating that the expected tokens were not encountered at the current position.

2. **Skip Newlines**:
   ```cpp
   skipNewlines();
   ```
   - After consuming the fat arrow or arrow token, the method skips any newlines that may follow. This ensures that the parser continues processing on the correct line after encountering the function definition.

3. **Determine Function Body Type**:
   ```cpp
   if (check(TokenType::LBRACE) || check(TokenType::INDENT))
   {
       auto body = parseBlock();
       LambdaExpr le;
       le.params = std::move(params);
       le.body = std::move(body);
       return std::make_unique<ASTNode>(std::move(le), ln);
   }
   ```
   - The method then checks if the next token is either an opening brace `{` or an indentation. If either condition is true, it assumes that the function body is a block statement. It calls `parseBlock()` to parse the block and constructs a `LambdaExpr` object with the parsed parameters and body. Finally, it wraps the `LambdaExpr` in an `ASTNode` and returns it.

4. **Expression Body Handling**:
   ```cpp
   else
   {
       auto expr = parseExpr();
       int eln = expr->line;
       auto retStmt = std::make_unique<ASTNode>(ReturnStmt{std::move(expr)}, eln);
       BlockStmt block;
       block.statements.push_back(std::move(retStmt));
       auto body = std::make_unique<ASTNode>(std::move(block), ln);
       LambdaExpr le;
       le.params = std::move(params);
       le.body = std::move(body);
       return std::make_unique<ASTNode>(std::move(le), ln);
   }
   ```
   - If the next token is neither an opening brace nor an indentation, the method assumes that the function body is a single expression. It parses this expression using `parseExpr()`. To handle the implicit return, it creates a `ReturnStmt` containing the parsed expression and stores its line number. A `BlockStmt` is then constructed, which holds only one statement: the `ReturnStmt`. This block statement is wrapped in an `ASTNode`, along with the parsed parameters, to form a `LambdaExpr`. Finally, the `LambdaExpr` is encapsulated within another `ASTNode` and returned.

## Edge Cases

- **Missing Fat Arrow or Arrow Token**: If the parser encounters a token sequence that does not start with `=>` or `->`, it will throw a `ParseError`.
- **Empty Function Body**: If the function body is empty (either due to missing braces or indentation), the parser will still correctly construct the AST by wrapping the empty body in a `BlockStmt`.

## Interactions with Other Components

- **Parameters Parsing**: Before calling `parseArrowFunction`, the parser should have already called `parseParams()` to extract the parameters of the arrow function into the `params` variable.
- **Line Number Tracking**: The method uses the `ln` parameter to store the line number where the arrow function starts. This line number is passed to various AST nodes to maintain accurate source code location information.
- **Error Handling**: The method interacts with the error handling mechanism provided by the compiler. If a parsing error occurs, it throws a `ParseError` with appropriate details, which is caught and handled by the higher-level error reporting system.

This implementation ensures that both block and expression-based arrow functions are correctly parsed and represented in the AST, facilitating further compilation steps.