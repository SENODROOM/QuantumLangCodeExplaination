# `parseArgList`

## Purpose

The `parseArgList` function is responsible for parsing a list of arguments that are enclosed within parentheses. These arguments can include positional arguments, keyword arguments, generator expressions, and unpacking of keyword arguments using the `**` syntax. The function ensures proper syntax and structure while building an Abstract Syntax Tree (AST) representation of the parsed arguments.

## Parameters

- None explicitly listed in the provided code snippet.

## Return Value

- Returns a `std::vector<ASTNodePtr>` containing pointers to the AST nodes representing each parsed argument.

## How It Works

1. **Expecting Parentheses**: The function starts by expecting an opening parenthesis (`(`). If not found, it throws an error indicating that an expected token was missing.

2. **Parsing Arguments**: Inside a loop, the function continues to parse arguments until it encounters a closing parenthesis (`)`).

3. **Handling Positional Arguments**: For each argument, it first checks if the argument is a keyword argument or a generator expression. If neither, it parses the argument as a regular expression.

4. **Handling Keyword Arguments**: If the current token is an identifier followed by an assignment operator (`=`), it skips the identifier part and directly uses the value of the expression following the assignment as the keyword argument.

5. **Handling Unpacking of Keyword Arguments**: If the current token is the `POWER` token (`**`), it consumes it and expects an expression. This expression is wrapped in a `UnaryExpr{"**", ...}` node to indicate that it should be treated as a spread operation during evaluation.

6. **Handling Generator Expressions**: If the current token is the `FOR` token, it expects a generator expression. The function reads variables involved in the generator and iterates over the iterable to construct the generator expression.

7. **Skipping Newlines**: Throughout the parsing process, the function calls `skipNewlines()` to ensure that any newlines encountered are properly skipped, maintaining the correct flow of parsing.

8. **Matching Commas**: After processing each argument, the function checks for a comma (`,`) to determine if there are more arguments to parse. If a comma is found, it continues; otherwise, it breaks out of the loop.

## Edge Cases

- **Missing Closing Parenthesis**: If the input string does not contain a matching closing parenthesis, the function will throw an error indicating an unexpected end of file.
  
- **Empty Argument List**: An empty argument list (i.e., `()`) results in an empty vector being returned, which represents a valid call without any arguments.

- **Invalid Syntax**: If the input contains invalid syntax such as mismatched parentheses, incorrect usage of `**` for unpacking, or improper placement of commas, the function will throw appropriate errors.

## Interactions With Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens that represent the input string. It consumes tokens from the global `tokens` vector and updates the `pos` index accordingly.

- **Error Handling**: Errors are handled through calls to functions like `expect()` and `consume()`. These functions manage the state of the parser and throw exceptions when encountering unexpected tokens or conditions.

- **AST Construction**: The parsed arguments are converted into AST nodes using classes like `ASTNode` and `UnaryExpr`. These nodes are then collected into a vector, which is returned at the end of the function.

- **Evaluation Context**: During the parsing of keyword arguments and unpacking operations, the function constructs AST nodes that reflect these operations. When the generated AST is evaluated, these nodes will trigger the corresponding behavior (e.g., spreading keyword arguments).

This comprehensive approach ensures that `parseArgList` can handle a wide range of argument structures, making it robust for various inputs in the Quantum Language compiler.