# `parseArrowFunction`

## Purpose

The `parseArrowFunction` method is designed to parse arrow functions within the Quantum Language compiler. Arrow functions offer a compact syntax for defining anonymous functions, which are frequently utilized for tasks such as creating callbacks or performing simple computations.

## Parameters

- **params**: A reference to a vector of `Parameter` objects representing the parameters of the arrow function being parsed.
- **ln**: An integer indicating the line number where the arrow function starts.

## Return Value

The method returns a unique pointer to an `ASTNode` object, encapsulating the parsed arrow function. The returned node contains a `LambdaExpr` object, which holds the parsed parameters and body of the arrow function.

## Detailed Explanation

### Parsing Syntax

1. **Token Matching**:
   - The method begins by checking if the current token is either `TokenType::FAT_ARROW` (i.e., `=>`) or `TokenType::ARROW` (i.e., `->`). If neither token is found, a `ParseError` is thrown, indicating that the expected tokens were not encountered at the current position.

2. **Skipping Newlines**:
   - After matching the arrow token, any newlines are skipped using the `skipNewlines()` function to ensure the parser continues processing on the correct line.

3. **Body Parsing**:
   - Depending on whether the next token is a `{` (indicating a block body) or an indentation (also indicating a block body), the method proceeds to parse the block using the `parseBlock()` function.
   - If the body is a single expression, the method calls `parseExpr()` to parse the expression. This expression is then wrapped inside a `ReturnStmt`, which represents an explicit return statement. The `ReturnStmt` is placed within a `BlockStmt`, forming an implicit return block. Finally, both the parameter list and the implicit return block are combined into a `LambdaExpr`, and a unique pointer to an `ASTNode` containing this `LambdaExpr` is returned.

### Edge Cases

- **Empty Function Body**: If the function body consists solely of a newline character, the parser will correctly handle it by skipping the newline and proceeding to parse the function body.
- **Misplaced Tokens**: If the expected tokens (`=>` or `->`) are not present, a `ParseError` will be thrown, providing clear feedback on the syntax error.

### Interactions with Other Components

- **Tokenizer**: The `parseArrowFunction` method relies on the tokenizer to identify and process individual tokens during the parsing process.
- **Expression Parser**: When encountering a single-expression body, the method uses the `parseExpr()` function to parse the expression and convert it into an AST node.
- **Block Parser**: For block bodies, the method utilizes the `parseBlock()` function to parse the statements within the block and construct an appropriate AST structure.

By carefully handling both single-expression and block bodies, the `parseArrowFunction` method ensures that the Quantum Language compiler can accurately parse and represent arrow functions in its abstract syntax tree (AST).