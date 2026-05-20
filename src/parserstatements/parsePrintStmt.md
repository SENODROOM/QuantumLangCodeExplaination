# `parsePrintStmt`

## Purpose

The `parsePrintStmt` function in the Quantum Language compiler is designed to parse print statements. It supports both simple print statements and more complex ones that can include optional keyword arguments such as `sep`, `end`, `file`, and `flush`.

## Parameters/Return Value

- **Parameters**:
  - None explicitly mentioned in the provided code snippet.
  
- **Return Value**:
  - The function returns an `ASTNodePtr` representing the parsed print statement node.

## How It Works

### Simple Print Statements

For simple print statements, the function parses a single expression and constructs an AST node for the print statement without any additional attributes.

### Complex Print Statements

For complex print statements, the function handles multiple expressions and optional keyword arguments. Here’s how it works:

1. **Initialization**:
   - The function initializes variables to store the line number (`ln`), whether a newline should be added at the end (`newline`), the separator string (`sep`), the end string (`end_str`), and a vector of AST nodes for the arguments (`args`).

2. **Parsing Parentheses**:
   - If the next token is an opening parenthesis (`(`), indicating a complex print statement, the function consumes the token and skips any newlines.
   
3. **Processing Arguments**:
   - Inside the parentheses, the function enters a loop where it processes each argument until it encounters a closing parenthesis (`)`).
     - **Keyword Arguments**: If the current token is an identifier followed by an assignment operator (`=`), it checks if the identifier is one of the supported keywords (`sep`, `end`, `file`, `flush`). Depending on the keyword, it consumes the corresponding value (which must be a string). For `sep` and `end`, it updates the respective strings. For `file` and `flush`, it consumes the expression but discards its value since these keywords are not used in the generated AST.
     - **Expressions**: If the current token is not a keyword, it parses an expression using the `parseExpr()` method and adds the resulting AST node to the `args` vector.
     
4. **Handling Commas**:
   - After processing each argument, the function skips any newlines and checks if the next token is a comma (`,`). If so, it continues processing the next argument. If not, it breaks out of the loop.

5. **Closing Parenthesis**:
   - The function expects a closing parenthesis (`)`). If it finds one, it consumes it. Otherwise, it throws an error.

6. **Post-Parentheses Processing**:
   - If there were no parentheses, the function directly parses the first expression and then continues to parse subsequent expressions separated by commas, adding them to the `args` vector.

7. **Constructing AST Node**:
   - Finally, the function constructs an AST node for the print statement using the collected arguments, separator, and end string. It returns this AST node.

## Edge Cases

- **Missing Closing Parenthesis**: If a complex print statement is missing a closing parenthesis, the function will throw an error indicating that a `')'` was expected.
- **Non-Literal Values for Keywords**: If a keyword argument is provided but its value is not a literal string, the function will still consume the expression but discard its value. This ensures that the parser remains robust even if unexpected input is encountered.
- **Empty Print Statement**: An empty print statement (i.e., just `print`) is not allowed and will result in an error.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: The function uses methods like `expect()` to handle errors gracefully, ensuring that the parser stops at the correct location when encountering invalid syntax.
- **Expression Parsing**: The function calls `parseExpr()` to parse individual expressions within the print statement. This interaction allows the parser to handle various types of expressions seamlessly.

Overall, `parsePrintStmt` is a crucial part of the Quantum Language compiler's parsing mechanism, enabling it to correctly interpret and construct AST nodes for print statements, which are essential for outputting information during program execution.