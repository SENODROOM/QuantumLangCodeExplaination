# `parseArgList`

## Purpose

The `parseArgList` function is designed to parse a list of arguments within parentheses, handling both positional arguments and keyword arguments. It also supports generator expressions and kwargs unpacking using the `**` operator.

## Parameters/Return Value

- **Parameters**: None explicitly listed in the provided code snippet, but it operates on global state (`pos`, `tokens`) and modifies them as it parses the argument list.
- **Return Value**: A `std::vector<ASTNodePtr>` containing pointers to the parsed AST nodes representing the arguments. Each node can be either a positional argument or a keyword argument.

## How It Works

1. **Expecting Parentheses**: The function starts by expecting an opening parenthesis `(`. If not found, it throws an error indicating that a parenthesis was expected.

2. **Parsing Arguments**: It then enters a loop that continues until it encounters a closing parenthesis `)` or reaches the end of the input.

3. **Handling Kwargs Unpacking**: 
   - If the next token is `TokenType::POWER` (which represents `**`), it consumes this token and expects an expression following it. This expression is wrapped in a `UnaryExpr` with the operator `"**"`, indicating that it should be treated as a spread operation during evaluation.
   - After parsing the expression, it checks if there is a comma `,` to separate arguments. If not, the loop breaks, assuming all arguments have been parsed.

4. **Handling Keyword Arguments**:
   - If the next token is an `Identifier`, it looks ahead to see if the subsequent tokens form a keyword argument (`name=expr`). If they do, it skips the identifier and the assignment operator `=`.
   - It then parses the expression associated with the keyword argument.

5. **Parsing Expressions**: For each argument, the function calls `parseExpr()` to parse the expression into an AST node.

6. **Handling Generator Expressions**:
   - If the next token is `TokenType::FOR`, it recognizes a generator expression. It then reads variables involved in the generator expression and expects an iterable expression after the `in`.

7. **Skipping Newlines**: Throughout the parsing process, the function uses `skipNewlines()` to handle any newlines encountered in the input, ensuring that the parser remains robust against formatting issues.

## Edge Cases

- **Empty Argument List**: If the argument list is empty (i.e., only parentheses are present), the function will return an empty vector without throwing an error.
- **Unmatched Parentheses**: If there are unmatched parentheses (an opening parenthesis without a corresponding closing one or vice versa), the function will throw an appropriate error.
- **Invalid Kwargs Syntax**: If a keyword argument is incorrectly formatted (e.g., missing the assignment operator `=`), the function will throw an error.
- **Generator Expression Without Iterable**: If a generator expression is used without specifying an iterable, the function will throw an error.

## Interactions With Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens (`tokens`) and advances through these tokens using the `consume()` function.
- **Error Handling**: Errors are handled using functions like `expect()` and `match()`, which check for specific token types and throw errors if they are not found.
- **Expression Parsing**: The function uses `parseExpr()` to parse individual expressions within the argument list, which may involve further recursion depending on the complexity of the expressions.
- **AST Construction**: The parsed arguments are stored as AST nodes in a vector, which can be used by other parts of the compiler for semantic analysis, code generation, etc.

This comprehensive approach ensures that `parseArgList` can accurately parse various forms of argument lists, making it a crucial component for the Quantum Language compiler.