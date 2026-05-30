# `parsePower`

## Description

The `parsePower` function is a key part of the parser in the Quantum Language compiler. It is responsible for interpreting and constructing Abstract Syntax Tree (AST) nodes that represent expressions using the power operator (`**`). This function ensures that the parsing process correctly handles expressions involving powers, particularly when dealing with nested power operations.

## Parameters/Return Value

### Parameters
- None

### Return Value
- Returns a unique pointer to an `ASTNode` object representing the parsed expression. If there is no power operation, it returns the result of `parseUnary()`.

## Edge Cases

1. **No Power Operator**: If the current token is not a power operator (`**`), the function simply returns the result of `parseUnary()`. This allows for the parsing of simpler expressions without interruption.
2. **Nested Powers**: The function uses recursion to handle nested power operations. For example, the expression `a ** b ** c` would be parsed as `a ** (b ** c)`, ensuring correct precedence and associativity.
3. **Invalid Tokens**: If at any point during the parsing of the power operation, an invalid token is encountered, the function will throw an exception or handle it appropriately based on the error handling strategy implemented in the compiler.

## Interactions with Other Components

- **parseUnary()**: This function is called before checking for the power operator. It parses expressions that may start with unary operators such as `-` or `+`.
- **consume()**: When the power operator is detected, this function consumes the token, advancing the parser's position to the next token in the input stream.
- **BinaryExpr**: An AST node type used to represent binary operations, including the power operation. The `BinaryExpr` constructor takes the operator string `"**"`, along with pointers to the left and right operands, which are constructed by recursively calling `parsePower()`.

## Why It Works This Way

The design of `parsePower` follows a recursive descent approach, which is common in parsers. By first parsing the left-hand side of the power operation with `parseUnary()`, the function sets up the initial operand. Then, it checks for the presence of the power operator using `check(TokenType::POWER)`. If found, it consumes the operator and recursively calls itself to parse the right-hand side, ensuring that the rightmost operand is evaluated first due to the right-associative nature of the power operator. Finally, it constructs an `ASTNode` with a `BinaryExpr` representing the power operation and returns it. This structure allows for the clear representation and evaluation of complex power expressions within the Quantum Language compiler.