# `parseAssignment`

## Purpose

The `parseAssignment` function is responsible for parsing assignment expressions within the Quantum Language compiler. It supports both Python-style and JavaScript/C-style ternary operators, ensuring accurate interpretation and parsing of these constructs.

## Functionality

The function begins by storing the current line number (`ln`) using the `current()` method. It then calls `parseOr()` to parse the left-hand side of the assignment expression.

Next, it checks if the next token is a `TokenType::IF`, indicating a potential Python-style ternary operator. To distinguish between a ternary operator and a list comprehension filter, it performs a lookahead:

- It increments a `depth` counter for each opening parenthesis, square bracket, or curly brace encountered.
- For each closing parenthesis, square bracket, or curly brace, it decrements the `depth` counter.
- If a `TokenType::ELSE` is found at the same depth as the initial `TokenType::IF`, it confirms that the expression is indeed a ternary operator.
- The function consumes the `TokenType::IF` token after confirming its presence.
  
If a `TokenType::ELSE` is confirmed, it parses the condition and the right-hand side expression (`elseExpr`). It then returns a unique pointer to a new `ASTNode` containing a `TernaryExpr` object with the parsed condition, left-hand side, and right-hand side expressions.

If no `TokenType::ELSE` is found, it assumes the expression might be a list comprehension filter. In such cases, it simply returns the result of `parseOr()`, deferring further parsing to `parseExpr()` or `parseListComp()`.

Finally, the function also checks for a JavaScript/C-style ternary operator (`condition ? thenExpr : elseExpr`). If this type of ternary operator is detected, it consumes the `TokenType::QUESTION` token, parses the `thenExpr`, expects a `TokenType::COLON`, and then parses the `elseExpr`. It returns a unique pointer to a new `ASTNode` containing a `TernaryExpr` object with the parsed condition, `thenExpr`, and `elseExpr`.

## Parameters/Return Value

- **Parameters**: None.
- **Return Value**: A unique pointer to an `ASTNode` representing the parsed assignment expression.

## Edge Cases

- **Empty Expression**: If the input stream is empty, the function should handle this gracefully without causing errors.
- **Syntax Errors**: The function should detect and report syntax errors, such as missing `else` or colon in ternary operators.
- **Nested Ternaries**: The function should correctly handle nested ternary operators, maintaining proper scope and precedence.

## Interactions with Other Components

The `parseAssignment` function interacts with several other components within the compiler:

- **Tokenizer**: It uses the tokenizer to retrieve the current token and perform lookahead operations.
- **Error Handling**: It reports syntax errors through the error handling mechanism of the compiler.
- **Abstract Syntax Tree (AST)**: It constructs AST nodes representing parsed expressions, which are used for further compilation and execution phases.

This comprehensive approach ensures that the Quantum Language compiler can accurately parse complex assignment expressions, supporting both Pythonic and traditional programming paradigms.