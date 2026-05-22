# parseArrowFunction

## Purpose

The `parseArrowFunction` method is designed to handle the parsing of arrow functions within the Quantum Language compiler. Arrow functions offer a compact and readable syntax for defining anonymous functions, which are often utilized for tasks such as creating callbacks or executing simple computations.

## Parameters/Return Value

- **Parameters**: 
  - `params`: A vector containing the parsed parameters of the arrow function. This parameter is passed by reference and modified during the parsing process.
  - `ln`: The line number where the arrow function starts. This is typically derived from the current token's location.

- **Return Value**:
  - Returns a unique pointer to an `ASTNode` representing the parsed arrow function. The `ASTNode` contains a `LambdaExpr`, which encapsulates the parameters and body of the arrow function.

## How It Works

1. **Token Consumption**:
   - The method first checks whether the next token is either `=>` (FAT_ARROW) or `->` (ARROW). If neither token is found, it throws a `ParseError` indicating that the expected tokens were not present at the current position.

2. **Skipping Newlines**:
   - After consuming the fat arrow or regular arrow token, the method skips any newline characters (`skipNewlines()`). This ensures that the parser moves past any whitespace following the arrow symbol.

3. **Parsing the Function Body**:
   - The method then determines whether the body of the arrow function is a block statement or a single expression.
     - If the body is a block statement (indicated by `{` or `INDENT`), it calls `parseBlock()` to parse the block and constructs a `LambdaExpr` with the parsed parameters and block body.
     - If the body is a single expression, it parses the expression using `parseExpr()`. To create an explicit return statement, it wraps the parsed expression in a `ReturnStmt` and adds this statement to a new `BlockStmt`. This block is then wrapped in another `ASTNode` to form the `body` of the `LambdaExpr`.

4. **Constructing the ASTNode**:
   - Regardless of whether the body was a block or a single expression, the method constructs a `LambdaExpr` object with the parsed parameters and body.
   - Finally, it returns a unique pointer to an `ASTNode` containing the `LambdaExpr`. The returned node includes the original line number (`ln`) to maintain context in the abstract syntax tree (AST).

## Edge Cases

- **Missing Fat Arrow or Regular Arrow Token**: If the next token is neither `=>` nor `->`, the method will throw a `ParseError`.
- **Empty Block Statement**: If the block statement is empty (i.e., `{}` or `INDENT` followed by nothing), the method will still construct a valid `LambdaExpr` with an empty block body.
- **Single Expression Body**: If the body consists of a single expression without an explicit return statement, the method will automatically add one.

## Interactions with Other Components

- **Tokenizer**: The method relies on the tokenizer to provide the sequence of tokens for parsing. It uses methods like `current()` to access the current token and `match()` and `check()` to verify token types.
- **Parser Blocks**: When encountering a block body, the method interacts with the `parseBlock()` function to handle the nested structure of statements within the block.
- **Abstract Syntax Tree (AST)**: The method constructs various nodes of the AST, including `LambdaExpr`, `BlockStmt`, and `ReturnStmt`, to represent the parsed structure of the arrow function.

By carefully handling different scenarios and ensuring proper interaction with other parts of the compiler, the `parseArrowFunction` method provides robust support for parsing arrow functions in the Quantum Language.