# `parsePrintStmt` Function

## Overview

The `parsePrintStmt` function is designed to parse print statements in the Quantum Language compiler's source code. It handles both simple and complex print statements, including those with keyword arguments such as `sep`, `end`, `file`, and `flush`.

## Parameters/Return Value

- **Parameters**: None explicitly listed in the provided snippet, but implied to be part of the class state or passed through method calls.
- **Return Type**: `ASTNodePtr`
  
The function returns an `ASTNodePtr` representing the parsed print statement node in the abstract syntax tree (AST).

## How It Works

### Parsing Simple Print Statements

If the current token is not a left parenthesis (`(`), the function assumes a simple print statement without any keyword arguments. It parses the expression following the `print` keyword and adds it to the `args` vector. The function continues parsing expressions separated by commas until it reaches the end of the line or encounters another token that indicates the end of the statement.

### Handling Keyword Arguments

For complex print statements with keyword arguments, the function first checks if the next token is a left parenthesis. If so, it consumes the left parenthesis and skips any newlines. Then, it enters a loop where it processes each argument:

- **Keyword Argument Detection**: If the current token is an identifier followed by an assignment operator (`=`) and one of the recognized keywords (`sep`, `end`, `file`, `flush`), the function consumes the keyword and the assignment operator. Depending on the keyword:
  - For `sep`, it expects a string literal and sets the `sep` variable accordingly.
  - For `end`, it also expects a string literal, sets the `end_str` variable, and disables the default newline behavior (`newline = false`).
  - For `file` and `flush`, it consumes the expression but discards its value since these arguments are not used in the AST.

- **Non-Key Argument Parsing**: If the current token is not a keyword argument, the function parses the expression and adds it to the `args` vector.

- **Loop Control**: After processing each argument, the function skips any newlines and checks if the next token is a comma. If a comma is found, it continues parsing the next argument. If not, it breaks out of the loop.

### Finalizing the Statement

After parsing all arguments, the function expects a right parenthesis (`)`). If the expected token is not found, it throws an error indicating the missing closing parenthesis.

## Edge Cases

- **Missing Parentheses**: If the print statement lacks parentheses, the function will throw an error when expecting the right parenthesis.
- **Invalid Keywords**: If a keyword argument is used incorrectly (e.g., not followed by an assignment operator or a non-string value for `sep` and `end`), the function will consume the invalid argument but continue parsing the rest of the statement.
- **Empty Arguments**: If there are no arguments between the parentheses, the `args` vector will remain empty.

## Interactions with Other Components

- **Token Stream**: The function interacts with the token stream managed by the parser class. It uses methods like `current()`, `consume()`, `skipNewlines()`, and `match()` to navigate through the tokens and determine their type.
- **Error Handling**: Errors during parsing are handled using methods like `expect()`. These errors can be related to missing tokens, incorrect usage of keywords, or unexpected data types.
- **Abstract Syntax Tree**: The parsed print statement is represented as an `ASTNodePtr`. This node is constructed based on the arguments and keyword values parsed by the function.

This comprehensive approach ensures that the `parsePrintStmt` function can handle various forms of print statements gracefully, providing robust support for debugging and logging within the Quantum Language compiler.