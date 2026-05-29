# `parseArgList`

## Purpose

The `parseArgList` function is designed to parse a list of arguments enclosed within parentheses. This includes handling positional arguments, keyword arguments, generator expressions, and unpacking of keyword arguments using the `**` syntax. The function ensures proper parsing and construction of an Abstract Syntax Tree (AST) representing the parsed arguments.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: A vector of ASTNodePtr objects representing the parsed arguments.

## How It Works

The function starts by expecting a left parenthesis `(`. It then enters a loop that continues until it encounters a right parenthesis `)` or reaches the end of the input. Within the loop, it handles different types of arguments:

1. **Unpacking Keyword Arguments (`**expr`)**:
   - If the next token is `TokenType::POWER`, indicating unpacking, the function consumes the `**` token and parses the expression following it.
   - The parsed expression is wrapped in a `UnaryExpr` node with the operator `"**"`. This indicates to the evaluation context that the expression should be treated as a spread operation.
   - The function then skips any newlines and checks if there is a comma `,` after the unpacking expression. If not, the loop breaks.

2. **Handling Keyword Arguments (`name=expr`)**:
   - If the next token is an identifier, the function checks if the subsequent tokens indicate a keyword assignment (`=`).
   - If a keyword assignment is found, the function consumes the identifier and the `=` token, skipping any intervening newlines.
   - The function then parses the expression following the `=` token, which represents the value of the keyword argument.

3. **Parsing Positional Arguments**:
   - For all other cases, the function assumes that the current token represents a positional argument.
   - It parses the expression corresponding to the positional argument.
   - The function then skips any newlines.

4. **Generator Expressions (`f(expr for var in iterable)`)**:
   - If the next token is `TokenType::FOR`, indicating the start of a generator expression, the function consumes the `for` token and reads variables involved in the generator.
   - Variables are read one at a time, consuming identifiers or CType keywords.
   - After reading the variables, the function expects a `TokenType::IN` token followed by an iterable expression.

## Edge Cases

- **Empty Argument List**: If the argument list is empty (i.e., only contains parentheses), the function returns an empty vector.
- **Trailing Comma**: If there is a trailing comma after the last argument, the function will still parse correctly and ignore the extra comma.
- **Nested Parentheses**: The function can handle nested parentheses within the argument list, ensuring that each set of parentheses is properly closed before moving on to the next set.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: The function uses error-handling mechanisms provided by the tokenizer to report issues such as missing closing parentheses or unexpected tokens.
- **Evaluation Context**: When constructing the AST nodes, the function may interact with the evaluation context to determine how to treat certain expressions, especially those involving unpacking operations.

This comprehensive approach allows `parseArgList` to accurately parse various forms of argument lists encountered in quantum language code, providing a robust foundation for further processing and interpretation.