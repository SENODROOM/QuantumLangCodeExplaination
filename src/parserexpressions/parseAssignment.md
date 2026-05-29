# `parseAssignment`

## Purpose

The `parseAssignment` function is designed to parse assignment expressions within the Quantum Language compiler. This function handles both Python-style and JavaScript/C-style ternary operators, ensuring that these constructs are accurately interpreted and parsed during the compilation process.

## Functionality

The primary functionality of `parseAssignment` involves parsing expressions that assign values to variables or properties. The function first parses the left-hand side of the assignment using the `parseOr()` method, which can handle various types of expressions including identifiers, array access, and property access.

After parsing the left-hand side, the function checks if the next token is either the keyword `if` (indicating a Python-style ternary operator) or a question mark (`?`) (indicating a JavaScript/C-style ternary operator). Depending on the type of ternary operator detected, the function proceeds as follows:

### Python-Style Ternary Operator

For Python-style ternary operators, the function consumes the `if` keyword and then parses the condition using the `parseOr()` method. It then looks ahead in the token stream to determine if an `else` clause exists. To do this, it increments a depth counter when encountering opening brackets, parentheses, or braces and decrements it when encountering corresponding closing brackets, parentheses, or braces. If the depth counter returns to zero and an `else` keyword is found, the function sets the `hasElse` flag to `true`. If the `hasElse` flag is set, the function consumes the `else` keyword and parses the right-hand side of the ternary expression using the `parseAssignment()` method. Finally, the function constructs a new AST node representing the ternary expression using the `TernaryExpr` struct and returns it.

### JavaScript/C-Style Ternary Operator

For JavaScript/C-style ternary operators, the function consumes the question mark (`?`) and then parses the `thenExpr` using the `parseExpr()` method. After parsing the `thenExpr`, the function expects a colon (`:`) to separate the `thenExpr` and the `elseExpr`. Once the colon is consumed, the function parses the `elseExpr` using the `parseAssignment()` method. Finally, the function constructs a new AST node representing the ternary expression using the `TernaryExpr` struct and returns it.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: Returns a unique pointer to an `ASTNode` object representing the parsed assignment expression.

## Edge Cases

- **Empty Expression**: If the input token stream is empty or contains only whitespace, the function may return an empty `ASTNode` or throw an exception depending on the implementation.
- **Missing Tokens**: If the input token stream is missing required tokens such as `else` or `colon`, the function will throw an exception indicating the expected token was not found.
- **Nested Expressions**: The function correctly handles nested ternary operators by adjusting the depth counter appropriately.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: The function uses error handling mechanisms provided by the compiler to report syntax errors and unexpected token sequences.
- **AST Construction**: The function constructs an abstract syntax tree (AST) node representing the parsed assignment expression, which is used by subsequent stages of the compiler for further processing.