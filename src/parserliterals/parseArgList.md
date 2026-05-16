# `parseArgList`

## Purpose

The `parseArgList` function is designed to parse a list of arguments enclosed within parentheses. This includes handling positional arguments, keyword arguments, generator expressions, and unpacking of keyword arguments using the `**` syntax. The function ensures proper parsing and constructs an Abstract Syntax Tree (AST) representing the parsed arguments.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: `std::vector<ASTNodePtr>` - A vector containing pointers to AST nodes representing each argument in the list.

## How It Works

1. **Expect Parentheses**: The function starts by expecting a left parenthesis (`(`). If not found, it throws an error indicating that a left parenthesis was expected.

2. **Initialize Argument List**: An empty vector `args` is initialized to store the parsed AST nodes of the arguments.

3. **Skip Newlines**: The function skips any newlines before beginning the parsing loop.

4. **Parse Arguments**: The function enters a loop that continues until a right parenthesis (`)`), end of file, or an unexpected token is encountered:
   - **Check for Keyword Unpacking (`**`)**: If the next token is `TokenType::POWER`, it indicates that the argument list is being unpacked using the `**` syntax. The function consumes the `**` token, parses the expression following it, and wraps it in a `UnaryExpr{"**", ...}` node. This node is then added to the `args` vector.
   - **Check for Keyword Arguments**: If the next token is an identifier (`TokenType::IDENTIFIER`), the function checks if the subsequent tokens form a keyword assignment (`name=`). If they do, the function skips the identifier and the assignment operator, then parses the expression following the equals sign. The parsed expression is added to the `args` vector.
   - **Parse Expressions**: For all other cases, the function parses an expression using the `parseExpr()` method and adds it to the `args` vector.
   - **Skip Newlines After Each Argument**: After parsing each argument, the function skips any newlines.

5. **Handle Comma Separators**: If the next token after an argument is a comma (`,`), the function consumes the comma and continues parsing the next argument. If a comma is not followed by another argument, the loop breaks.

6. **Final Check for Right Parenthesis**: Before exiting the loop, the function checks if the next token is a right parenthesis (`)`). If not, it throws an error indicating that a right parenthesis was expected.

7. **Return Parsed Arguments**: Finally, the function returns the vector `args` containing the parsed AST nodes of the arguments.

## Edge Cases

- **Empty Argument List**: The function handles an empty argument list correctly by simply returning an empty vector.
- **Trailing Commas**: The function allows trailing commas in the argument list but ignores them.
- **Nested Parentheses**: The function can handle nested parentheses within the argument list, ensuring correct parsing of inner expressions.
- **Invalid Tokens**: If an invalid token is encountered during parsing, the function throws appropriate errors.

## Interactions With Other Components

- **Tokenizer**: The `parseArgList` function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: Errors related to missing parentheses, incorrect token types, and unexpected characters are handled by throwing exceptions.
- **AST Construction**: The function constructs AST nodes for each parsed argument, which are used by other components of the compiler for further processing.

This comprehensive approach ensures that the `parseArgList` function accurately parses complex argument lists and constructs a robust AST representation for further compilation steps.