# `parsePower` Function

## Purpose
The `parsePower` function is designed to parse expressions involving the power operator (`**`). It recursively processes the expression on its left and then consumes the power token before parsing the expression on its right. This ensures that the power operation is right-associative, meaning that expressions like `a ** b ** c` will be evaluated as `a ** (b ** c)`.

## Parameters
This function takes no explicit parameters. It relies on global state managed by the parser, such as `current()` which returns the current token being processed, and `consume()` which advances the token stream to the next token.

## Return Value
The function returns a unique pointer to an `ASTNode`, representing the parsed binary expression node. If there is no power operator in the current context, it simply returns the result of parsing the unary expression.

## Edge Cases
1. **No Power Operator**: If the current token is not a power operator (`TokenType::POWER`), the function returns the result of parsing the unary expression directly.
2. **Nested Powers**: The function handles nested power operations correctly due to its recursive nature. For example, `a ** b ** c` results in an AST structure where `c` is applied first, followed by `b`, and finally `a`.

## Interactions with Other Components
- **`parseUnary`**: This function is called initially within `parsePower`. It parses expressions that can start with a unary operator or a primary expression.
- **`check`**: This utility function checks if the current token matches the specified type (`TokenType::POWER`).
- **`consume`**: This function advances the token stream to the next token after consuming the current one.
- **`ASTNode`**: Represents nodes in the abstract syntax tree (AST). In this case, it's used to create a new binary expression node with the operator `"**"`.
- **`BinaryExpr`**: A subclass of `ASTNode` specifically designed to represent binary arithmetic operations. It holds the operator string and pointers to the left and right operands.

## Detailed Explanation
Here’s a step-by-step breakdown of how `parsePower` operates:

1. **Parse Unary Expression**: The function starts by calling `parseUnary()`, which attempts to parse the initial part of the expression. This could be a number, variable, parentheses, etc., depending on the current token.

2. **Check for Power Token**: After obtaining the left operand, the function checks if the next token is a power operator (`TokenType::POWER`) using the `check` function.

3. **Consume Power Token**: If a power token is found, the function consumes it using `consume()`. This moves the token stream forward so that subsequent parsing can proceed without encountering the power token again.

4. **Recursive Parse Right Operand**: The function then calls itself recursively to parse the right operand of the power expression. Since power operations are right-associative, this call effectively handles any nested powers in the expression.

5. **Create Binary Expression Node**: Once both operands are parsed, the function creates a new `ASTNode` containing a `BinaryExpr` object. This node represents the power operation between the left and right operands. The line number of the power token is passed to ensure accurate error reporting or debugging information.

6. **Return Result**: Finally, the function returns the newly created `ASTNode`.

This design allows `parsePower` to handle complex expressions involving multiple levels of power operations efficiently and correctly.