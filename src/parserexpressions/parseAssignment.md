# `parseAssignment`

## Purpose

The `parseAssignment` function is designed to parse assignment expressions in the Quantum Language compiler. It handles both Python-style and JavaScript/C-style ternary operators, as well as standard assignments.

## Functionality

### Python Ternary Operator

For Python-style ternary operators (`expr IF condition ELSE other_expr`), the function checks if the next token is `TokenType::IF`. If it is, the function consumes the `IF` token and parses the condition using `parseOr()`. It then looks ahead to ensure that there is an `ELSE` statement before encountering a closing bracket, parenthesis, or newline. If an `ELSE` statement is found, the function consumes it and parses the `elseExpr` using `parseAssignment()`. Finally, it constructs and returns a unique pointer to a `TernaryExpr` AST node containing the parsed condition, left-hand side, and right-hand side expressions.

### JavaScript/C Ternary Operator

For JavaScript/C-style ternary operators (`condition ? thenExpr : elseExpr`), the function checks if the next token is `TokenType::QUESTION`. If it is, the function consumes the `?` token and parses the `thenExpr` using `parseExpr()`. The function then expects a colon (`:`) token and consumes it. After parsing the colon, the function parses the `elseExpr` using `parseAssignment()`.

### Standard Assignments

If neither Python nor JavaScript/C-style ternary operators are detected, the function simply parses the left-hand side of the assignment using `parseOr()` and returns it as is.

## Parameters and Return Value

- **Parameters**: None
- **Return Value**: Returns a unique pointer to an `ASTNode` representing the parsed assignment expression.

## Edge Cases

- **Missing `ELSE`**: If a Python ternary operator is encountered but there is no corresponding `ELSE` statement before a closing bracket, parenthesis, or newline, the function assumes it might be part of a list comprehension filter and returns only the left-hand side expression.
- **Incorrect Syntax**: If the syntax of a ternary operator is incorrect (e.g., missing colon after question mark), the function throws an error indicating the expected token.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: The function uses the `expect` method to handle errors related to unexpected token types.
- **Recursive Parsing**: The function calls itself recursively to parse nested ternary expressions or to handle the right-hand side of the assignment when dealing with ternary operators.

This function plays a crucial role in the parser by correctly identifying and handling different types of assignment expressions, ensuring that the Quantum Language compiler can accurately interpret and process these expressions during compilation.