# `parsePrintStmt`

## Purpose

The `parsePrintStmt` function is designed to parse print statements in the Quantum Language compiler. It handles both simple print statements and those that include keyword arguments such as `sep`, `end`, `file`, and `flush`.

## Parameters/Return Value

- **Parameters**:
  - None explicitly defined within the provided code snippet.

- **Return Value**:
  - An `ASTNodePtr` representing the parsed print statement node in the Abstract Syntax Tree (AST).

## How It Works

### Parsing Simple Print Statements

If the current token is not an opening parenthesis (`(`), indicating a simple print statement without any keyword arguments, the function proceeds as follows:

1. **Line Number**: The line number of the current token is stored in the variable `ln`.
2. **Expression Parsing**: The function calls `parseExpr()` to parse the expression following the `print` keyword and adds it to the `args` vector.
3. **Comma Handling**: The function continues parsing expressions separated by commas until it reaches the end of the input or encounters a non-comma token.

### Parsing Print Statements with Keyword Arguments

If the current token is an opening parenthesis (`(`), indicating a print statement with keyword arguments, the function proceeds as follows:

1. **Line Number**: The line number of the current token is stored in the variable `ln`.
2. **Opening Parenthesis Consumption**: The function consumes the opening parenthesis using `consume()`.
3. **Skipping Newlines**: The function skips any newlines using `skipNewlines()`.
4. **Argument Parsing Loop**: The function enters a loop where it parses each argument inside the parentheses:
   - **Keyword Argument Detection**: If the current token is an identifier followed by an assignment operator (`=`) and one of the keywords `sep`, `end`, `file`, or `flush`, the function processes the keyword argument accordingly:
     - For `sep`: If the next token is a string literal, the function updates the `sep` variable with the string value. Otherwise, it discards the non-literal expression.
     - For `end`: If the next token is a string literal, the function updates the `end_str` variable with the string value and sets `newline` to `false`. This prevents the default newline behavior when printing. Otherwise, it discards the non-literal expression.
     - For `file` and `flush`: These keywords are currently handled by consuming and discarding the associated expression.
   - **Non-Key Argument Parsing**: If the current token is not a keyword argument, the function parses it as a regular expression using `parseExpr()` and adds it to the `args` vector.
   - **Comma Skipping**: After processing each argument, the function skips any newlines and checks for a comma. If a comma is found, it continues parsing the next argument. If not, it breaks out of the loop.
5. **Closing Parenthesis Expectation**: The function expects a closing parenthesis (`)`). If it doesn't find one, it throws an error.

### Edge Cases

- **Missing Closing Parenthesis**: If a print statement includes keyword arguments but lacks a closing parenthesis, the function will throw an error.
- **Non-Literal Values for Keywords**: If a keyword argument like `sep` or `end` is not followed by a string literal, the function will discard the non-literal expression.
- **Empty Print Statement**: If the `print` keyword is followed immediately by a closing parenthesis without any arguments, the function will still create a valid AST node for the print statement.

## Interactions with Other Components

The `parsePrintStmt` function interacts with several other components of the Quantum Language compiler:

- **Token Stream**: It uses the global `tokens` stream to access and consume tokens.
- **Error Handling**: It utilizes functions like `expect` and `consume` to handle errors and advance the token stream.
- **Abstract Syntax Tree (AST)**: It constructs an `ASTNodePtr` representing the parsed print statement, which can be used by subsequent stages of compilation for further processing.

This function is crucial for correctly interpreting and compiling print statements in the Quantum Language, ensuring that the output format matches the specified requirements.