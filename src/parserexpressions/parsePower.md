# `parsePower`

## Description

The `parsePower` function is an essential component of the parser within the Quantum Language compiler. Its primary role is to interpret and construct Abstract Syntax Tree (AST) nodes representing expressions that utilize the power operator (`**`). This function employs a recursive descent parsing technique to handle expressions involving multiple levels of exponentiation, ensuring correct precedence through its right-associativity.

## Parameters

- None

## Return Value

- Returns a unique pointer to an `ASTNode` object representing the parsed expression tree. If the current token is not a power operator, it returns the result of `parseUnary()`.

## Edge Cases

1. **No Power Operator**: If the current token is not a power operator (`**`), the function simply returns the result of `parseUnary()`, effectively skipping any potential power operations in the expression.
2. **Nested Exponentiation**: The function handles nested exponentiations correctly due to its right-associative nature. For example, the expression `a ** b ** c` is interpreted as `a ** (b ** c)`.

## Interactions with Other Components

- **parseUnary()**: This function is called at the beginning of `parsePower()` to parse the base expression before encountering the power operator. It ensures that the base expression is properly constructed before proceeding with the power operation.
- **check(TokenType::POWER)**: This function checks whether the current token in the input stream is a power operator (`**`). If it is, the function proceeds to parse the right-hand side of the expression.
- **consume()**: When a power operator is encountered, this function consumes the token from the input stream, advancing the parser to the next token.
- **std::make_unique<ASTNode>()**: This function creates a new `ASTNode` object with a binary expression type (`BinaryExpr`) and assigns it the line number where the power operator was found. The node represents the entire power expression, including both the base and the exponent.

## Implementation Details

The implementation of `parsePower()` follows a typical recursive descent pattern:

1. **Base Case**: Start by calling `parseUnary()` to parse the base expression. This could be a variable, literal, or another expression.
2. **Recursive Case**: Check if the current token is a power operator using `check(TokenType::POWER)`. If it is:
   - Record the current line number (`ln`).
   - Consume the power operator token.
   - Recursively call `parsePower()` again to parse the exponent expression. This recursion ensures that exponentiation is handled right-associatively.
   - Create a new `ASTNode` with a binary expression type (`"**"`), moving ownership of the base and exponent AST nodes into it.
3. **Return**: If no power operator is found, simply return the result of `parseUnary()`.

This design allows `parsePower()` to seamlessly integrate with other parts of the parser, handling complex expressions involving multiple levels of exponentiation while maintaining correct precedence.