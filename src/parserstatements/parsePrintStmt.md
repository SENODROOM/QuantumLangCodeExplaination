# `parsePrintStmt`

## Purpose

The `parsePrintStmt` function in the Quantum Language compiler is responsible for parsing print statements. It handles both simple print statements and more complex ones that may include optional keyword arguments such as `sep`, `end`, `file`, and `flush`.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet, but it likely takes parameters related to the context of parsing, such as the current token position, token stream, and possibly an error reporting mechanism.

- **Return Value**:
  - Returns an `ASTNodePtr` representing the parsed print statement node. The node includes the arguments passed to the print function and any keyword arguments (`sep`, `end`, `file`, `flush`) that were specified.

## How It Works

The function starts by recording the current line number using `int ln = current().line;`. It then initializes variables to handle the optional keyword arguments (`sep`, `end_str`, `newline`) and a vector to store the arguments passed to the print function (`args`).

### Handling Parenthesized Arguments

If the next token is an opening parenthesis (`TokenType::LPAREN`), indicating a potentially complex print statement with keyword arguments, the function proceeds as follows:

1. **Consume Parenthesis**: The function consumes the opening parenthesis using `consume();`.
2. **Skip Newlines**: Any newlines encountered after the opening parenthesis are skipped using `skipNewlines();`.
3. **Parse Arguments**: The function enters a loop where it continues to parse arguments until it encounters a closing parenthesis (`TokenType::RPAREN`) or reaches the end of the input. Inside the loop:
   - **Check for Keyword Arguments**: If the current token is an identifier followed by an assignment operator (`=`), it identifies the keyword argument (`sep`, `end`, `file`, `flush`). Depending on the keyword, it parses the corresponding expression:
     - For `sep`, if the next token is a string literal, it sets `sep` to the value of the string. Otherwise, it calls `parseExpr()` to consume and discard the expression.
     - For `end`, if the next token is a string literal, it sets `end_str` to the value of the string and disables the default newline behavior by setting `newline` to `false`. Otherwise, it calls `parseExpr()` to consume and discard the expression.
     - For `file` and `flush`, it always calls `parseExpr()` to consume and discard the expressions since these keywords do not affect the output directly.
   - **Regular Arguments**: If the current token is not a keyword argument, it parses the expression as a regular argument and adds it to the `args` vector.
4. **Expect Closing Parenthesis**: After exiting the loop, the function expects a closing parenthesis using `expect(TokenType::RPAREN, "Expected ')'");`.

### Handling Non-Parenthesized Arguments

If the next token is not an opening parenthesis, indicating a simple print statement without keyword arguments, the function proceeds as follows:

1. **Parse First Argument**: The function parses the first expression as the primary argument and adds it to the `args` vector.
2. **Loop for Additional Comma-Separated Arguments**: The function enters another loop where it continues to parse additional comma-separated arguments until it reaches the end of the input. Each argument is added to the `args` vector.

### Final Steps

After parsing all arguments, the function constructs an ASTNodePtr representing the print statement. This node includes the recorded line number, the boolean `newline`, the values of `sep` and `end_str`, and the list of arguments (`args`).

## Edge Cases

- **Empty Print Statement**: If there are no arguments between the parentheses, the function should still be able to parse the empty print statement correctly.
- **Missing Closing Parenthesis**: If the closing parenthesis is missing, the function should report an error and stop parsing.
- **Incorrect Keyword Usage**: If a keyword argument is used incorrectly (e.g., without an assignment operator or with an invalid type), the function should report an error and continue parsing the rest of the arguments.
- **Mixed Positioning**: The function should handle cases where keyword arguments are mixed with positional arguments in any order.

## Interactions with Other Components

- **Token Stream**: The function interacts with the token stream to identify and consume tokens based on their type.
- **Error Reporting**: The function uses mechanisms like `expect()` to report errors when expected tokens are not found.
- **Expression Parsing**: The function relies on `parseExpr()` to parse individual expressions within the print statement.
- **Abstract Syntax Tree (AST)**: The function constructs an ASTNodePtr to represent the parsed print statement, which can then be further processed by other parts of the compiler.

This comprehensive approach ensures that the `parsePrintStmt` function accurately handles various scenarios and integrates seamlessly with the rest of the compiler's components.