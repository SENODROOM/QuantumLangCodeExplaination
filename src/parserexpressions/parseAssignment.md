# `parseAssignment`

## Purpose

The `parseAssignment` function is designed to parse assignment expressions within the Quantum Language compiler. This function handles both Python-style and JavaScript/C-style ternary operators, ensuring that these constructs are accurately interpreted and parsed during the compilation process.

## Parameters/Return Value

- **Parameters**:
  - None explicitly mentioned in the provided code snippet.

- **Return Value**:
  - Returns a unique pointer to an `ASTNode` representing the parsed assignment expression. The type of the `ASTNode` can either be a simple `AssignmentExpr` or a more complex `TernaryExpr`.

## How It Works

The function starts by parsing the left-hand side of the assignment using the `parseOr()` method. Depending on whether it encounters a Python-style `IF` keyword or a JavaScript/C-style question mark (`?`), it proceeds differently:

### Python-Style Ternary Operator

For Python-style ternary operators, the function checks if the next token is `TokenType::IF`. If it is, it looks ahead to ensure that the structure is indeed a ternary expression and not a list comprehension filter. This is done by counting the depth of parentheses, brackets, and braces to determine if an `ELSE` keyword appears before a closing delimiter. Once confirmed as a ternary expression, it consumes the `IF` keyword, parses the condition, expects the `ELSE` keyword, and finally parses the right-hand side expression. The result is wrapped in a `TernaryExpr` AST node.

### JavaScript/C-Style Ternary Operator

For JavaScript/C-style ternary operators, the function checks if the next token is `TokenType::QUESTION`. If it is, it consumes the question mark, parses the condition, expects the colon (`:`) after the condition, and then parses the right-hand side expression. The result is also wrapped in a `TernaryExpr` AST node.

If neither style of ternary operator is encountered, the function simply returns the parsed left-hand side expression as an `AssignmentExpr`.

## Edge Cases

- **Empty Expression**: If the input stream is empty or contains invalid syntax, the function may throw exceptions or return null pointers depending on the implementation details.
- **Misplaced Keywords**: If the `IF` or `QUESTION` keywords are misplaced or not followed by their respective conditions or expressions, the function will raise errors indicating expected tokens.
- **Nested Expressions**: The function correctly handles nested ternary expressions by maintaining the depth count when looking ahead.

## Interactions With Other Components

The `parseAssignment` function interacts with several other components within the compiler:

- **Tokenizer**: To retrieve the current token and move through the token stream.
- **ASTBuilder**: To construct the abstract syntax tree (AST) nodes representing the parsed expressions.
- **Error Handling**: To report syntax errors and unexpected token types to the user.

This function ensures that the Quantum Language compiler can handle various forms of conditional assignments, making the language more flexible and expressive for quantum programming tasks.