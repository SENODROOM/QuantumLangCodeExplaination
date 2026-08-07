# `parseArgList`

## Purpose

The `parseArgList` function is designed to parse a list of arguments that are enclosed within parentheses. This function handles various types of arguments including positional arguments, keyword arguments, generator expressions, and unpacking of keyword arguments using the `**` syntax. It ensures correct parsing by managing the state of whether it is currently inside a call argument list (`inCallArgList`) and properly handling newlines within the argument list.

## Parameters

- None

## Return Value

- A vector of `ASTNodePtr` representing the parsed arguments.

## How It Works

1. **Expect LPAREN**: The function starts by expecting a left parenthesis token (`(`). If not found, it throws an error indicating that a left parenthesis was expected.

2. **Initialize Argument List**: It initializes an empty vector `args` to store the parsed arguments.

3. **Skip Newlines**: The function skips any newline tokens to ensure proper parsing of multi-line input.

4. **Set Call Argument List State**: It temporarily sets the `inCallArgList` flag to `true` to indicate that it is now inside a call argument list. This helps in correctly parsing keyword arguments and unpacking operations.

5. **Parse Arguments Loop**: The function enters a loop that continues until it encounters a right parenthesis token (`)`) or reaches the end of the input:
   - **Check for RPAREN**: If a right parenthesis is encountered, the loop breaks.
   - **Check for atEnd**: If the parser has reached the end of the input without encountering a right parenthesis, the loop also breaks.
   - **Track Current Line Number**: It tracks the line number where the current argument starts.
   - **Skip Newlines Again**: Skips any newline tokens again within the loop.
   - **Handle Kwargs Unpacking**: If the next token is a `POWER` token (`**`), it consumes the `POWER` token and parses the expression that follows. The parsed expression is wrapped in a `UnaryExpr` with the operator `"**"`, indicating that it is a keyword argument unpacking operation. This wrapped expression is then added to the `args` vector.
     - **Consume POWER Token**: Consumes the `POWER` token (`**`).
     - **Parse Expression**: Parses the expression following the `POWER` token.
     - **Wrap in UnaryExpr**: Wraps the parsed expression in a `UnaryExpr` with the operator `"**"`.
     - **Break on Missing Comma**: If a comma is not found immediately after the unpacking operation, the loop breaks.
   - **Handle Keyword Arguments**: If the next token is an `IDENTIFIER`, it checks if the subsequent token is an `ASSIGN` token (`=`). If both conditions are met, it parses the keyword argument:
     - **Save Keyword Name**: Consumes the identifier token and saves its value as the keyword name.
     - **Skip Newlines Again**: Skips any newline tokens again within the loop.
     - **Consume ASSIGN Token**: Consumes the `ASSIGN` token (`=`).
     - **Parse Expression**: Parses the expression following the `ASSIGN` token.
     - **Create AssignExpr**: Creates an `AssignExpr` object with the operator `"="`, the keyword name as the target, and the parsed expression as the value.
     - **Add to Args Vector**: Adds the `AssignExpr` object to the `args` vector.

6. **Restore Call Argument List State**: After exiting the loop, it restores the original state of the `inCallArgList` flag.

7. **Return Parsed Arguments**: Finally, it returns the vector of parsed arguments.

## Edge Cases

- **Missing Right Parenthesis**: If the input does not contain a matching right parenthesis, the function will stop parsing at the end of the input and return the parsed arguments up to that point.
- **Extra Commas**: Extra commas between arguments are handled gracefully, but trailing commas are not allowed.
- **Nested Calls**: The function manages the state of being inside a call argument list to avoid mis-parsing of nested calls or groupings.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: It uses error-handling mechanisms provided by the tokenizer to report errors such as missing parentheses.
- **Abstract Syntax Tree (AST)**: The parsed arguments are represented as nodes in the AST, which allows for further processing during compilation.
- **evalCall**: When dealing with unpacking operations (`**`), the function wraps them in a `UnaryExpr` to inform the evaluation phase that these are spread operations.

This comprehensive approach ensures that the `parseArgList` function can handle complex argument lists accurately and efficiently, providing a solid foundation for further parsing and compilation steps.