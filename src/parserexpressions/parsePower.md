# `parsePower`

## Description

The `parsePower` function is a crucial part of the parser in the Quantum Language compiler, responsible for interpreting expressions that use the power operator (`**`). It leverages a recursive descent parsing technique to handle nested power operations, ensuring correct evaluation order and syntax handling.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed expression tree.

## How It Works

The function starts by calling `parseUnary()`, which parses the left-hand side of the power expression. If the next token is a `TokenType::POWER` (indicating the presence of the power operator), the function consumes the token and recursively calls itself to parse the right-hand side of the expression. This recursive call ensures that the function correctly handles right-associativity, meaning higher powers are evaluated first.

Once both sides of the expression are parsed, the function constructs a new `ASTNode` using the `BinaryExpr` constructor, passing the string `"**"` as the operator, and moves the parsed nodes into it. The line number (`ln`) is also captured and passed to the `ASTNode` constructor to maintain context about where in the source code the expression was found.

If there is no power operator present after the initial call to `parseUnary()`, the function simply returns the result of `parseUnary()`.

## Edge Cases

- **No Power Operator**: If the expression does not contain the power operator (`**`), the function will return the result of `parseUnary()` directly.
- **Nested Powers**: The function can handle expressions with multiple levels of nested powers, such as `(2 ** 3) ** 4`. It ensures that these are evaluated correctly according to the rules of right-associativity.
- **Invalid Syntax**: If the syntax is invalid (e.g., missing operand or misplaced operator), the function may throw exceptions or produce unexpected results depending on how the error handling is implemented in the surrounding parser logic.

## Interactions with Other Components

- **Tokenizer**: The `parsePower` function relies on the tokenizer to provide tokens for parsing. It uses functions like `current()` and `consume()` to interact with the tokenizer's state.
- **Error Handling**: While not explicitly shown in the provided snippet, the parser likely includes mechanisms to handle errors gracefully when encountering invalid syntax or other issues during parsing.
- **AST Construction**: The `parsePower` function contributes to building the Abstract Syntax Tree (AST). It creates `ASTNode` objects representing binary expressions involving the power operator, which are then used by higher-level parts of the parser to construct more complex expressions and statements.

Overall, the `parsePower` function plays a vital role in accurately parsing quantum language expressions that involve the power operator, leveraging recursion to handle nested operations and ensure syntactic correctness.