# `parseArrowFunction`

## Purpose

The `parseArrowFunction` function is designed to parse arrow functions within the Quantum Language compiler. Arrow functions provide a concise syntax for defining anonymous functions that inherit their parent's scope and automatically return expressions without requiring an explicit `return` statement.

## Functionality

### Parsing Syntax

The function begins by consuming either the `=>` (FAT_ARROW) or `->` (ARROW) token, which indicates the start of an arrow function. If neither token is found, a `ParseError` is thrown, signaling that the expected syntax was not encountered.

```cpp
if (!match(TokenType::FAT_ARROW) && !match(TokenType::ARROW))
    throw ParseError("Expected '=>' or '->'", current().line, current().col);
```

### Skipping Newlines

After identifying the arrow function syntax, any newlines are skipped to ensure that the parser continues processing on the correct line.

```cpp
skipNewlines();
```

### Parsing Function Body

The next step involves determining whether the function body consists of a block (`{}`) or a single expression. This decision is made using the `check` function to verify the presence of `{`, `INDENT`, or both.

#### Block Body

If a block body is detected (either through an opening brace `{` or indentation), the function proceeds to parse the block using the `parseBlock()` method. The parsed parameters and block are then encapsulated into a `LambdaExpr` object, which represents the arrow function.

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

#### Single Expression Body

If a single expression body is detected, the function parses the expression using the `parseExpr()` method. To maintain the automatic return behavior of arrow functions, the parsed expression is wrapped inside a `ReturnStmt` object, which is then placed within a `BlockStmt`. Finally, the `BlockStmt` and its associated parameters are encapsulated into a `LambdaExpr`.

```cpp
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
```

### Parameters and Return Value

- **Parameters**: The function expects parameters to have been previously parsed and stored in the `params` variable. These parameters are moved into the `LambdaExpr`.
- **Return Value**: The function returns a unique pointer to an `ASTNode` containing the parsed `LambdaExpr`. The `LambdaExpr` includes both the parameters and the function body.

### Edge Cases

- **Missing Syntax**: If the function encounters a token other than `=>` or `->`, it throws a `ParseError` indicating that the expected syntax was missing.
- **Empty Body**: While the existing code does not explicitly handle empty bodies, the parser should ideally be able to recognize and handle such cases gracefully.

### Interactions with Other Components

- **Lexical Analysis**: The function relies on the lexical analyzer (`current()`) to identify tokens and determine the position in the source code where errors occur.
- **Parsing Blocks**: When encountering a block body, the function interacts with the `parseBlock()` method, which is responsible for parsing the statements within the block.
- **Parsing Expressions**: For single-expression bodies, the function uses the `parseExpr()` method to parse the expression itself.

By understanding these aspects, developers can better appreciate how `parseArrowFunction` contributes to the overall parsing process of the Quantum Language compiler, ensuring that arrow functions are correctly identified and handled according to their syntax and semantics.