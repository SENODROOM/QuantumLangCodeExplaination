# `parseArgList`

## Purpose

The `parseArgList` function is designed to parse a list of arguments enclosed within parentheses. This includes handling positional arguments, keyword arguments, generator expressions, and unpacking of keyword arguments using the `**` syntax. The function ensures proper parsing and construction of an Abstract Syntax Tree (AST) representing the argument list.

## Parameters

- None

## Return Value

- Returns a `std::vector<ASTNodePtr>` containing pointers to the parsed AST nodes representing each argument in the list.

## How It Works

1. **Expect LPAREN**: The function starts by expecting a left parenthesis (`(`). If not found, it throws an error indicating that a left parenthesis was expected.

2. **Initialize Argument List**: An empty vector `args` is initialized to store pointers to the parsed AST nodes.

3. **Skip Newlines**: The function calls `skipNewlines()` to skip any newline characters before starting the argument parsing loop.

4. **Argument Parsing Loop**: The function enters a loop that continues until a right parenthesis (`)` is encountered or the end of the input is reached:
   - **Track Line Number**: For each argument, the line number where it begins is tracked.
   - **Skip Newlines Again**: The function skips any newline characters at the start of the loop iteration.
   - **Check for Keyword Unpacking (`**`)**: If the next token is a `POWER` token (`**`), indicating keyword unpacking, the function consumes the `POWER` token and parses the following expression. The parsed expression is wrapped in a `UnaryExpr{"**", ...}` node to indicate that it should be evaluated as a spread operation. This node is then added to the `args` vector.
   - **Check for Keyword Arguments**: If the next token is an `IDENTIFIER`, the function checks if it is followed by an `ASSIGN` token (`=`), indicating a keyword argument. If so, the function consumes both the identifier and the assignment token, skipping any newlines between them, and then consumes the value expression associated with the keyword argument. This value expression is added to the `args` vector.
   - **Parse Expression**: If none of the above conditions are met, the function parses the current expression using the `parseExpr()` method and adds the resulting AST node to the `args` vector.
   - **Skip Newlines After Expression**: The function skips any newline characters after parsing an expression.
   - **Check for Comma Separators**: If the next token is a `COMMA`, the function consumes it and continues to the next argument. If not, the loop breaks, indicating that all arguments have been parsed.

5. **Return Parsed Arguments**: Once the loop completes, the function returns the `args` vector containing pointers to the parsed AST nodes representing each argument in the list.

## Edge Cases

- **Empty Parentheses**: If only a pair of empty parentheses is provided, the function will return an empty vector.
- **Trailing Commas**: Trailing commas within the argument list are allowed and will simply be ignored.
- **Mixed Types**: The function can handle a mix of positional arguments, keyword arguments, and generator expressions within the same argument list.
- **Nested Expressions**: Nested expressions within the argument list are correctly parsed and handled.

## Interactions With Other Components

- **Tokenizer**: The `parseArgList` function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: The function uses error-handling mechanisms provided by the tokenizer to report errors such as missing closing parentheses or unexpected tokens.
- **Expression Parser**: The function calls `parseExpr()` to parse individual expressions within the argument list, which is implemented in another part of the compiler.
- **Abstract Syntax Tree Construction**: The parsed AST nodes are constructed using the `ASTNode` class and added to the `args` vector, which is used to build the final AST representation of the function call or similar construct.

This comprehensive approach ensures that the `parseArgList` function can accurately parse complex argument lists and construct corresponding AST nodes, facilitating further processing in the compilation pipeline.