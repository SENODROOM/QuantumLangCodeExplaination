# `parseArrowFunction`

## Purpose

The `parseArrowFunction` method is responsible for parsing arrow functions within the Quantum Language compiler. Arrow functions provide a concise syntax for defining anonymous functions, commonly used for tasks like creating callbacks or performing simple computations.

## Parameters and Return Value

- **Parameters**:
  - `params`: A vector of parameter names that have already been parsed before calling `parseArrowFunction`.

- **Return Value**:
  - Returns a unique pointer to an `ASTNode` representing the parsed arrow function. The `ASTNode` contains either a `LambdaExpr` or a `BlockStmt`, depending on whether the function body consists of a single expression or a block of statements.

## How It Works

1. **Consume Arrow Token**: 
   - The method first checks if the current token is either a FAT_ARROW (`=>`) or ARROW (`->`). If not, it throws a `ParseError` indicating that the expected tokens were not found.

2. **Skip Newlines**:
   - After consuming the arrow token, the method skips any newline characters using `skipNewlines()` to ensure proper parsing of the function body.

3. **Determine Function Body Type**:
   - The method then checks if the next token is either an LBRACE (`{`) or an INDENT (`\t`). This determines if the function body is a block of statements or a single expression.
   
4. **Parse Block Body**:
   - If the function body is a block, the method calls `parseBlock()` to parse the block of statements. It then constructs a `LambdaExpr` object, setting its `params` to the previously parsed parameters and `body` to the parsed block. Finally, it returns a unique pointer to an `ASTNode` containing the `LambdaExpr`.
   
5. **Parse Single Expression Body**:
   - If the function body is a single expression, the method parses the expression using `parseExpr()`. It then wraps this expression in a `ReturnStmt` to create an implicit return statement. This `ReturnStmt` is added to a new `BlockStmt`, which is itself wrapped in an `ASTNode`. The `LambdaExpr` is constructed with the previously parsed parameters and this new `BlockStmt` as its body. The method finally returns a unique pointer to an `ASTNode` containing the `LambdaExpr`.

## Edge Cases

- **Missing Arrow Token**: If neither `=>` nor `->` is encountered, a `ParseError` will be thrown.
- **Empty Function Body**: If the function body is empty after skipping newlines, the method should handle this gracefully without throwing errors.
- **Mixed Syntax**: The method assumes consistent use of either blocks or single expressions. Mixing these syntaxes could lead to unexpected behavior or errors.

## Interactions with Other Components

- **Parsing Context**: `parseArrowFunction` operates within the context of a larger parsing process, likely initiated by a call to another parser method that recognizes the start of a function definition.
- **Token Matching**: It uses methods like `match()` and `check()` to identify and consume specific tokens during parsing.
- **Block Parsing**: When encountering a block body, it interacts with the `parseBlock()` method to parse the individual statements within the block.
- **Expression Parsing**: For single-expression bodies, it relies on the `parseExpr()` method to parse the expression.

This method plays a crucial role in accurately interpreting and constructing abstract syntax trees (ASTs) for arrow functions in the Quantum Language, facilitating further compilation steps.