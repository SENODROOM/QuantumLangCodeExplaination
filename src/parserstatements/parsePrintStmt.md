# `parsePrintStmt`

## Purpose

The `parsePrintStmt` function in the Quantum Language compiler is designed to parse print statements. It handles both basic print statements and those that include optional keyword arguments such as `sep`, `end`, `file`, and `flush`.

## Parameters/Return Value

- **Parameters**:
  - None explicitly listed in the provided code snippet.
  
- **Return Value**:
  - Returns an `ASTNodePtr` representing the parsed print statement node.

## How It Works

### Basic Print Statement Parsing

If the current token is not a left parenthesis (`(`), the function assumes a basic print statement. It parses the expression following the print keyword and adds it to the `args` vector. The function continues to parse expressions separated by commas until it encounters the end of the line or reaches the end of the input.

### Handling Keyword Arguments

If the current token is a left parenthesis (`(`), the function enters a more complex parsing mode to handle keyword arguments. Here's how it works:

1. **Consume the Left Parenthesis**: The function consumes the left parenthesis token.
2. **Skip Newlines**: It skips any newlines that might appear after the left parenthesis.
3. **Parse Arguments Inside Parentheses**:
   - A loop runs until it encounters a right parenthesis (`)`).
   - If the current token is an identifier followed by an equals sign (`=`), it checks if the identifier is one of the valid keywords (`sep`, `end`, `file`, `flush`). 
     - For `sep` and `end`, if the next token is a string literal, it consumes the string and updates the corresponding variable (`sep` or `end_str`). If the next token is not a string literal, it calls `parseExpr()` to consume and discard the expression.
     - For `file` and `flush`, it simply calls `parseExpr()` to consume and discard the expression.
   - If the current token is not an identifier followed by an equals sign, it assumes it's an argument expression and pushes it onto the `args` vector.
4. **Skip Newlines After Comma Separators**: The function skips any newlines that might appear after comma separators.
5. **Break Loop on Non-Comma Token**: The loop breaks when it encounters a token that is not a comma.
6. **Expect Right Parenthesis**: Finally, the function expects a right parenthesis token and throws an error if it doesn't find one.

### Edge Cases

- **Missing Right Parenthesis**: If the function encounters a left parenthesis without a matching right parenthesis, it will throw an error indicating that a closing parenthesis was expected.
- **Non-Literal String for `sep` and `end`**: If a non-string literal is used for the `sep` or `end` keyword, the function will still consume the expression but ignore its value.
- **Empty Argument List**: If there are no arguments inside the parentheses, the function will push an empty expression onto the `args` vector.

## Interactions with Other Components

The `parsePrintStmt` function interacts with several other components within the compiler:

- **Tokenizer**: It uses the tokenizer to get the current token and check for different types of tokens (e.g., `TokenType::LPAREN`, `TokenType::RPAREN`, `TokenType::IDENTIFIER`, `TokenType::ASSIGN`, `TokenType::STRING`, etc.).
- **Error Reporting**: If the function encounters unexpected tokens or syntax errors, it reports these errors using the error reporting mechanism.
- **Abstract Syntax Tree (AST)**: It constructs an AST node representing the print statement, which includes the arguments and any keyword arguments specified.

Overall, the `parsePrintStmt` function provides robust support for parsing print statements in the Quantum Language, handling both basic and advanced use cases with proper error checking and AST construction.