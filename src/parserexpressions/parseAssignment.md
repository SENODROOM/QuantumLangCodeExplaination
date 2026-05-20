# `parseAssignment`

## Purpose

The `parseAssignment` function is designed to parse assignment expressions within the Quantum Language compiler. This function handles both Python-style and JavaScript/C-style ternary operators, ensuring that the syntax is correctly interpreted and parsed.

## Functionality

The primary functionality of `parseAssignment` involves parsing expressions based on whether they follow Python or JavaScript/C style ternary syntax rules. The function starts by parsing an initial expression using the `parseOr()` method. Depending on the next token, it decides whether to handle a Python-style ternary (`expr IF condition ELSE other_expr`) or a JavaScript/C-style ternary (`condition ? thenExpr : elseExpr`).

For Python-style ternaries, the function checks if the next token is `TokenType::IF`. It then performs a lookahead to determine if the subsequent structure contains an `ELSE` clause. This lookahead ensures that the parser can distinguish between a ternary operator and a list comprehension filter, which might use similar syntax structures but require different interpretations.

If an `ELSE` clause is found, the function consumes the `IF`, parses the condition, expects the `ELSE` keyword, and finally parses the `elseExpr`. These elements are combined into a `TernaryExpr` AST node, representing the ternary operation.

For JavaScript/C-style ternaries, the function checks if the next token is `TokenType::QUESTION`. Upon finding this token, it consumes it and proceeds to parse the `thenExpr`. Following the `thenExpr`, the function expects the colon (`:`) and then parses the `elseExpr`. These parsed elements form another `TernaryExpr` AST node.

In scenarios where neither Python nor JavaScript/C ternary syntax is detected, the function simply returns the initially parsed expression (`left`). This allows for flexibility in handling different types of assignment expressions without requiring additional syntax checks.

## Parameters

- **None**: The function does not take any explicit parameters. Instead, it relies on global state and the current position in the token stream to perform its operations.

## Return Value

The function returns a unique pointer to an `ASTNode` object. For ternary expressions, it constructs a `TernaryExpr` containing the parsed condition, left-hand side expression, and right-hand side expression. In cases where a ternary is not detected, it returns the parsed left-hand side expression as is.

## Edge Cases

- **Empty Expression**: If the input stream is empty or does not contain a valid assignment expression, the function may return an incomplete or incorrect AST node.
- **Syntax Errors**: The function includes error checking mechanisms to ensure that the ternary structure is properly formed. If expected tokens are missing or incorrectly placed, it raises appropriate exceptions or warnings.
- **Nested Ternaries**: While the function primarily focuses on simple ternary expressions, it should gracefully handle nested ternaries by maintaining proper depth tracking during the lookahead phase.

## Interactions with Other Components

- **Token Stream**: The function interacts directly with the global token stream to retrieve and consume tokens. This requires careful synchronization to avoid conflicts with other parsing functions.
- **AST Construction**: After parsing the necessary components, the function constructs an `ASTNode` using the parsed data. This node is then used by higher-level components to generate executable code or further analyze the program structure.
- **Error Handling**: The function incorporates error handling logic to manage unexpected situations such as missing tokens or malformed expressions. This ensures robustness against invalid inputs in the token stream.

Overall, `parseAssignment` plays a crucial role in the Quantum Language compiler by accurately interpreting and parsing various assignment expressions, thereby facilitating the generation of correct and efficient executable code.