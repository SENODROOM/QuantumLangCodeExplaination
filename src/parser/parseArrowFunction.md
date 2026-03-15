# `parseArrowFunction`

## Purpose

The `parseArrowFunction` function is responsible for parsing arrow functions in the Quantum Language compiler. Arrow functions are a shorthand syntax used to define anonymous functions that capture variables from their surrounding scope.

## Functionality

1. **Consume Fat Arrow or Regular Arrow**: The function first checks if the next token is either a fat arrow (`=>`) or a regular arrow (`->`). If neither is found, it throws a `ParseError` indicating that an expected arrow operator was not encountered.

2. **Skip Newlines**: After consuming the arrow operator, the function skips any newline characters using `skipNewlines()` to ensure proper parsing of the function's body.

3. **Determine Body Type**:
   - **Block Body**: If the next token is either an opening brace (`{`) or an indentation (indicating a multi-line block), the function calls `parseBlock()` to parse the entire block as the body of the lambda expression.
   - **Expression Body**: If the next token is neither an opening brace nor an indentation, it means the body is a single expression. In this case, the function parses the expression using `parseExpr()`, wraps it in an explicit `ReturnStmt` to create an implicit return block, and then constructs a `LambdaExpr` with the parsed parameters and the new block.

4. **Construct LambdaExpr**: Regardless of whether the body is a block or an expression, the function constructs a `LambdaExpr`. This struct contains two main fields:
   - `params`: A vector of parameter nodes representing the parameters of the lambda function.
   - `body`: An AST node representing the body of the lambda function.

5. **Create and Return ASTNode**: Finally, the function creates an `ASTNode` containing the constructed `LambdaExpr` and returns it. This `ASTNode` represents the complete parsed arrow function in the abstract syntax tree (AST).

## Parameters and Return Value

- **Parameters**:
  - None explicitly listed in the function signature. However, it relies on the `params` variable, which should have been initialized before calling `parseArrowFunction`.

- **Return Value**:
  - Returns a `std::unique_ptr<ASTNode>` pointing to an `ASTNode` representing the parsed arrow function. This node includes both the parameters and the body of the lambda function.

## Edge Cases

- **Missing Arrow Operator**: If the input does not contain an arrow operator (`=>` or `->`), the function will throw a `ParseError`.
- **Empty Body**: If the body of the arrow function is empty (either due to missing braces or indentation), the function may still parse successfully depending on the implementation details of `parseBlock()` and `parseExpr()`.
- **Single Statement vs. Multi-Line Block**: The function correctly handles both single-expression bodies and multi-line block bodies, ensuring that the appropriate AST structure is created.

## Interactions with Other Components

- **Parsing Context**: This function operates within a larger parsing context where `params` has already been populated by another part of the parser (likely `parseParams()`).
- **Token Matching**: It uses `match()` and `check()` methods to identify and consume tokens, interacting closely with the lexer component.
- **Block Parsing**: When encountering a block body, it calls `parseBlock()`, which likely involves parsing multiple statements enclosed in curly braces `{}`.
- **Expression Parsing**: For expression bodies, it calls `parseExpr()`, which would handle the parsing of individual expressions.
- **Return Statements**: When wrapping an expression in an implicit return block, it creates `ReturnStmt` objects, demonstrating interaction with the statement handling component of the compiler.

This function plays a crucial role in translating the syntactic sugar provided by arrow functions into a structured form in the AST, facilitating further compilation steps such as semantic analysis and code generation.