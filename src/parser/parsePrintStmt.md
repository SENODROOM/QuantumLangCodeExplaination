# `parsePrintStmt` Function

## Overview

The `parsePrintStmt` function is a crucial component of the Quantum Language compiler responsible for parsing print statements. This function ensures that the compiler can correctly interpret and handle various forms of print statements, including those with optional keyword arguments like `sep`, `end`, `file`, and `flush`. The primary goal of this function is to construct an Abstract Syntax Tree (AST) node representing the print statement, which can then be used for further compilation or interpretation phases.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the provided snippet, but implied through the use of member functions and variables such as `current()`, `tokens`, `pos`, etc.

- **Return Value**:
  - An `ASTNodePtr` pointing to the newly constructed AST node representing the parsed print statement.

## How It Works

The function begins by retrieving the line number (`ln`) where the current token is located. It initializes several variables:
- `newline`: A boolean indicating whether a newline should be added at the end of the print output.
- `sep`: A string representing the separator between items to be printed, initially set to a space (" ").
- `end_str`: A string representing the character(s) to append at the end of the print output, initially set to a newline ("\n").
- `args`: A vector to store the expressions that will be printed.

The function checks if the next token is an opening parenthesis (`(`). If it is, it consumes the token and skips any newlines that might follow. It then enters a loop that continues until it encounters a closing parenthesis (`)`), a comma followed by another expression, or reaches the end of the input.

Inside the loop, it first checks if the current token is an identifier followed by an assignment operator (`=`). If so, it identifies the keyword argument (`sep`, `end`, `file`, or `flush`). Depending on the keyword, it consumes the corresponding value (if it is a string) or discards the value (if it is not a literal).

If the current token is not a keyword argument, it assumes it is an expression to be printed and adds it to the `args` vector using the `parseExpr()` function.

After processing each item, the function skips any newlines and checks if a comma follows. If a comma is found, it continues to process the next item. If no more items are available or a closing parenthesis is encountered, the loop breaks.

Finally, the function expects a closing parenthesis (`)`) and constructs an `ASTNodePtr` representing the parsed print statement. This node includes the line number, the separator, the end string, and the list of expressions to be printed.

## Edge Cases

- **Empty Print Statement**: If the print statement contains only parentheses without any arguments, the function will still create an AST node with default values for `sep`, `end`, and `newline`.
- **Non-Literal Values for Keyword Arguments**: If a keyword argument (`sep`, `end`) is assigned a non-literal value, the function will consume the value but discard it since these arguments must be strings.
- **Incorrect Token Sequence**: If the sequence of tokens is incorrect (e.g., missing closing parenthesis), the function will raise an error using the `expect()` function, ensuring that the compiler stops processing and reports the issue.

## Interactions with Other Components

The `parsePrintStmt` function interacts closely with other parts of the compiler:
- **Tokenizer**: It uses the tokenizer to retrieve the current token (`current()`) and check subsequent tokens (`check()`).
- **Error Handling**: It leverages the error handling mechanism to report syntax errors using the `expect()` function.
- **Expression Parsing**: It calls the `parseExpr()` function to parse individual expressions within the print statement.
- **AST Construction**: It constructs an AST node using the parsed information, which can be used by other compiler components for further processing.

Overall, the `parsePrintStmt` function plays a vital role in accurately interpreting print statements in the Quantum Language, ensuring that the compiler can generate appropriate AST nodes for these statements.