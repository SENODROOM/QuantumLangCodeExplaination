# `parseArrowFunction`

## Purpose

The `parseArrowFunction` method is responsible for parsing arrow functions within the Quantum Language compiler. Arrow functions provide a concise syntax for defining anonymous functions, commonly used for tasks like creating callbacks or performing simple computations.

## Parameters and Return Value

- **Parameters**: 
  - `params`: A vector of parameter nodes representing the arguments of the arrow function being parsed.
  - `ln`: The line number where the arrow function starts.

- **Return Value**:
  - Returns a unique pointer to an ASTNode containing the parsed lambda expression (`LambdaExpr`).

## How It Works

1. **Consume Arrow Syntax**:
   - The method first checks if the current token is either `TokenType::FAT_ARROW` (=>) or `TokenType::ARROW` (->). If not, it throws a `ParseError` indicating that the expected tokens were not found at the current position.

2. **Skip Newlines**:
   - After consuming the arrow syntax, the method calls `skipNewlines()` to ensure that any newlines following the arrow are skipped over.

3. **Determine Function Body Type**:
   - The method then checks if the next token is either a `{` (indicating a block body) or an indentation (also indicating a block body). If either condition is true, it parses the block using the `parseBlock()` method.

4. **Create Lambda Expression**:
   - If a block body is detected, the method constructs a `LambdaExpr` object. This object includes the parsed parameters and the block body.
   - The method returns a unique pointer to an `ASTNode` containing the `LambdaExpr`.

5. **Handle Single Expression Body**:
   - If a single expression body is detected (i.e., there is no block), the method parses the expression using the `parseExpr()` method.
   - It then creates a `ReturnStmt` node containing the parsed expression and wraps this statement in a `BlockStmt`.
   - Another `LambdaExpr` is constructed with the parsed parameters and the implicit return block.
   - Finally, the method returns a unique pointer to an `ASTNode` containing the `LambdaExpr`.

## Edge Cases

- **Missing Arrow Syntax**: If neither `TokenType::FAT_ARROW` nor `TokenType::ARROW` is encountered, the method will throw a `ParseError`.
- **Empty Block Body**: If an empty block `{}` is provided after the arrow, the method will still construct a valid `LambdaExpr` with an empty block body.
- **Single Expression Body**: If a single expression is provided without a block, the method correctly handles it by wrapping the expression in an implicit return block.

## Interactions with Other Components

- **Tokenizer**: The method relies on the tokenizer to identify the correct tokens (`FAT_ARROW`, `ARROW`, `LBRACE`, etc.) during parsing.
- **Expression Parser**: For single-expression bodies, the method uses the `parseExpr()` function to parse the expression into an ASTNode.
- **Block Parser**: For block bodies, the method uses the `parseBlock()` function to parse the statements within the block into an ASTNode.
- **AST Construction**: The method constructs various ASTNodes such as `LambdaExpr`, `BlockStmt`, and `ReturnStmt` to represent different parts of the arrow function in the abstract syntax tree.

By handling both block and single-expression bodies, `parseArrowFunction` ensures flexibility in how arrow functions can be defined within the Quantum Language, making it easier for developers to write concise and readable code.