# `parsePrintStmt`

## Purpose

The `parsePrintStmt` function in the Quantum Language compiler is responsible for parsing print statements. It handles both simple print statements and more complex ones that may include optional keyword arguments such as `sep`, `end`, `file`, and `flush`. This function ensures that the syntax of print statements is correctly interpreted and constructs an appropriate Abstract Syntax Tree (AST) node to represent the parsed statement.

## Parameters/Return Value

- **Parameters**:
  - None explicitly mentioned in the provided snippet, but based on typical usage, it likely takes a reference to the current token position (`pos`) and possibly other context information.
  
- **Return Value**:
  - Returns an `ASTNodePtr` representing the parsed print statement. If there are errors during parsing, it may return a null pointer or throw an exception.

## How It Works

### Simple Print Statements

For simple print statements without any keyword arguments, the function consumes the expression following the `print` keyword and adds it to the list of arguments. It then continues to consume expressions separated by commas until it reaches the end of the line or encounters a different statement type.

### Complex Print Statements with Keyword Arguments

For complex print statements that include keyword arguments, the function first checks if the next token is a left parenthesis (`(`). If so, it enters a loop where it processes each argument within the parentheses. Inside the loop:

- **Keyword Argument Detection**: The function detects if the current token is an identifier followed by an equals sign (`=`), indicating a keyword argument. Supported keywords are `sep`, `end`, `file`, and `flush`.
  
- **Handling Keyword Arguments**:
  - For the `sep` keyword, if the next token is a string literal, it updates the `sep` variable with the string's value. Otherwise, it calls `parseExpr()` to consume and discard the expression.
  - For the `end` keyword, if the next token is a string literal, it updates the `end_str` variable with the string's value and sets `newline` to `false` because `end=` overrides the default behavior of appending a newline character at the end. Similar to `sep`, it discards non-string expressions.
  - For the `file` and `flush` keywords, the function calls `parseExpr()` to consume and discard any associated expressions since these arguments typically require file objects or boolean values which are not directly supported in the print statement syntax.

- **Non-Key Argument Handling**: If the current token is not a keyword argument, it assumes it is an expression to be printed and calls `parseExpr()` to add it to the list of arguments.

- **Loop Control**: After processing each argument, the function skips any newlines and checks if a comma follows. If a comma is found, it continues to process the next argument. If no comma is found, it breaks out of the loop.

- **Closing Parenthesis**: Once all arguments have been processed, the function expects a right parenthesis (`)`).

### Edge Cases

- **Missing Right Parenthesis**: If a keyword argument is detected but the corresponding right parenthesis is missing, the function will raise an error indicating the expected closing parenthesis.
- **Invalid Expressions**: If an invalid expression is encountered after a keyword like `sep` or `end`, the function will consume the expression but discard it, allowing the parser to continue with subsequent arguments or the main body of the code.
- **Empty Arguments List**: If no arguments are provided in the print statement, the function will still construct an AST node with an empty list of arguments.

## Interactions with Other Components

The `parsePrintStmt` function interacts closely with the rest of the parser, particularly with the `parseExpr()` function, which is used to parse individual expressions within the print statement. Additionally, it uses utilities like `consume()`, `expect()`, and `match()` to navigate through the tokens and validate the syntax of the print statement.

This function is crucial for accurately interpreting and constructing the AST for print statements, ensuring that the quantum language programs can execute their intended output operations correctly.