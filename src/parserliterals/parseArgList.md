# `parseArgList`

## Purpose

The `parseArgList` function is designed to parse a list of arguments enclosed within parentheses. This includes handling positional arguments, keyword arguments, generator expressions, and unpacking of keyword arguments using the `**` syntax. The function ensures proper parsing and construction of an abstract syntax tree (AST) representing the parsed arguments.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: A `std::vector<ASTNodePtr>` containing pointers to AST nodes representing the parsed arguments.

## How It Works

1. **Expecting Parentheses**: The function starts by expecting a left parenthesis (`(`). If not found, it throws an error indicating that a left parenthesis was expected.

2. **Parsing Arguments**: Inside a loop, it continuously parses arguments until a right parenthesis (`)` is encountered or the end of the input is reached.

3. **Handling Keyword Argument Unpacking (`**`)**:
   - If the next token is `TokenType::POWER`, it indicates a keyword argument unpacking operation (`**expr`). 
   - The function consumes the `**` token and then parses the expression following it.
   - To distinguish this from regular positional arguments, it wraps the parsed expression in a `UnaryExpr` node with the operator `"**"`.
   - After adding the wrapped expression to the `args` vector, it checks if there is a comma (`,`), which would indicate more arguments. If not, it breaks out of the loop.

4. **Handling Regular Keyword Arguments**:
   - If the next token is an `IDENTIFIER`, it might be followed by a keyword assignment (`=`).
   - The function skips any newlines before and after the identifier and the assignment token.
   - It consumes the identifier and the assignment token, then parses the expression following the assignment.
   - Since the goal is to handle only the value part of the keyword argument, the identifier itself is ignored.

5. **Parsing Expressions**:
   - For each argument, the function calls `parseExpr()` to parse the expression and adds it to the `args` vector.
   - It also handles any trailing newlines after parsing an expression.

6. **Handling Generator Expressions**:
   - If the next token is `TokenType::FOR`, it indicates a generator expression.
   - The function consumes the `for` token and then reads variables involved in the generator.
   - Variables can be identifiers or CType keywords.
   - After reading the variables, it expects an `IN` token followed by another expression, which represents the iterable part of the generator.
   - The function constructs an appropriate AST node for the generator expression and adds it to the `args` vector.

7. **Skipping Newlines**: Throughout the parsing process, the function uses `skipNewlines()` to ignore any extraneous newlines in the input.

## Edge Cases

- **Empty Argument List**: If the argument list is empty (i.e., immediately followed by a right parenthesis), the function will return an empty vector without throwing an error.
- **Trailing Comma**: If there is a trailing comma at the end of the argument list, the function will stop parsing at that point and not include the extra comma in the result.
- **Nested Parentheses**: The function correctly handles nested parentheses by continuing to parse arguments until the corresponding right parenthesis is encountered.

## Interactions With Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: Errors such as missing closing parentheses or invalid syntax are handled appropriately by throwing exceptions.
- **Abstract Syntax Tree Construction**: The parsed arguments are added to an AST, which is used by subsequent stages of compilation to generate executable code.

This comprehensive approach ensures that `parseArgList` can accurately parse various types of arguments and construct a robust AST representation, facilitating further processing in the quantum language compiler.