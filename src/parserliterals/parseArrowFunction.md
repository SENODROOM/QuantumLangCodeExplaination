# parseArrowFunction

## Purpose

The `parseArrowFunction` method is designed to handle the parsing of arrow functions within the Quantum Language compiler. Arrow functions offer a more concise syntax compared to traditional function declarations, making them ideal for quick and simple operations such as creating callbacks or performing basic transformations.

## Parameters/Return Value

- **Parameters**: 
  - `params`: A vector containing the parsed parameters of the arrow function.
  - `ln`: The line number where the arrow function starts.

- **Return Value**:
  - Returns a unique pointer to an `ASTNode`, which represents the abstract syntax tree node corresponding to the parsed arrow function.

## How It Works

1. **Token Matching**:
   - The method first checks if the next token matches either `TokenType::FAT_ARROW` (i.e., `=>`) or `TokenType::ARROW` (i.e., `->`). If neither token is found, it throws a `ParseError` indicating that the expected tokens were not encountered at the current position.

2. **Skipping Newlines**:
   - After matching the fat or thin arrow, the method skips any newline characters (`skipNewlines()`) to ensure proper parsing of the function's body.

3. **Parsing Function Body**:
   - The method then determines whether the function body is a block or a single expression.
     - If the next token is either `{` (indicating a block) or `INDENT` (indicating an indented block), it calls `parseBlock()` to parse the entire block of statements as the body of the arrow function.
     - If the next token is not a block delimiter, it assumes the body is a single expression. In this case, it parses the expression using `parseExpr()`.

4. **Creating ASTNode**:
   - Regardless of whether the body is a block or a single expression, the method constructs a `LambdaExpr` object. This object contains the parsed parameters and the function body.
   - If the body was a single expression, it wraps the expression in a `ReturnStmt` to create an implicit return statement. Then, it creates a `BlockStmt` containing just this return statement.
   - Finally, it constructs an `ASTNode` representing the lambda expression and returns it.

## Edge Cases

- **Empty Arrow Function**: If there is no body after the arrow symbol, the parser will throw an error because it expects either a block or an expression.
- **Single Parameter Without Parentheses**: While not strictly an arrow function feature, the parser should handle cases where a single parameter is provided without parentheses, e.g., `(x) => x + 1`. However, in the given code snippet, it seems the parser expects parameters to always be enclosed in parentheses.
- **Implicit Return**: When a single expression is used as the body, the parser automatically wraps it in a return statement, which might lead to unexpected behavior if the user intends to perform multiple actions within the function.

## Interactions with Other Components

- **Tokenizer**: The `parseArrowFunction` method relies on the tokenizer to identify the correct tokens (e.g., `=>`, `{`, `INDENT`, etc.) during parsing.
- **Expression Parser**: For single-expression bodies, the method uses the `parseExpr` function to parse the expression into an ASTNode.
- **Block Parser**: For block bodies, the method utilizes the `parseBlock` function to parse the entire block of statements.

This method plays a crucial role in the Quantum Language compiler by providing a streamlined way to define anonymous functions, enhancing readability and conciseness in the codebase.