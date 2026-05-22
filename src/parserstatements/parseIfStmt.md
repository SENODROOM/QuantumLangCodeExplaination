# `parseIfStmt` Function

## Purpose
The `parseIfStmt` function in the Quantum Language compiler is designed to parse an `if` statement from the token stream and construct the corresponding Abstract Syntax Tree (AST). It supports both traditional `if` statements and the more modern `if-with-initializer` syntax introduced in C++17.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: An `ASTNodePtr` representing the parsed `if` statement node.

## How It Works
The function begins by consuming the first token, which should be the keyword `if`. Depending on whether the next tokens indicate the presence of an initializer (`auto` followed by a variable name and an assignment expression), the function will handle parsing differently.

### Traditional `if` Statement
For a traditional `if` statement without an initializer:
1. The function checks if the next token is an opening parenthesis `(`.
2. Consumes the opening parenthesis.
3. Parses the condition expression using `parseExpr()`.
4. Expects a closing parenthesis `)` and consumes it.
5. Matches a colon `:` indicating the start of the body or statement block.
6. Skips any newlines before parsing the body or statement block using `parseBodyOrStatement()`.
7. Optionally parses an `else` branch by checking if the next token is `else`, and then parsing either another `if` statement or a body/statement block as the `else` content.
8. Constructs an `IfStmt` node with the parsed condition and body/else branches.

### `if-with-initializer` Statement
For an `if-with-initializer` statement:
1. The function checks if the next two tokens are `(` and `auto`, respectively.
2. Consumes the opening parenthesis and the `auto` keyword.
3. Consumes the variable name following `auto`.
4. Expects an assignment operator `=` and consumes it.
5. Parses the initialization expression using `parseExpr()`.
6. Expects a closing parenthesis `)` and consumes it.
7. Matches a colon `:` indicating the start of the body or statement block.
8. Skips any newlines before parsing the body or statement block using `parseBodyOrStatement()`.
9. Optionally parses an `else` branch by checking if the next token is `else`, and then parsing either another `if` statement or a body/statement block as the `else` content.
10. Constructs a `VarDecl` node for the initialized variable.
11. Constructs an `Identifier` node for the variable name used in the condition.
12. Constructs an `IfStmt` node with the identifier as the condition and the previously constructed variable declaration as part of its scope.

## Edge Cases
- **Missing Parentheses**: If the parentheses around the condition expression are missing, the parser will throw an error.
- **Invalid Assignment Operator**: If the assignment operator `=` is replaced with a comparison operator `==` within the `if-with-initializer`, the parser will treat it as a traditional `if` statement.
- **Misplaced Else Clause**: If the `else` clause is misplaced (e.g., not immediately following the `if` statement), the parser will throw an error.

## Interactions With Other Components
- **Token Stream Consumption**: The function consumes tokens from the global `tokens` vector, advancing the position `pos` accordingly.
- **Error Handling**: Utilizes functions like `expect()` and `match()` to ensure correct token sequence and report errors when expected tokens are not found.
- **Parsing Expressions and Statements**: Relies on `parseExpr()` and `parseBodyOrStatement()` to parse the condition expression and the body or statement block of the `if` statement.
- **Abstract Syntax Tree Construction**: Builds an AST by creating nodes such as `VarDecl`, `Identifier`, and `IfStmt` and linking them together based on the parsed structure of the `if` statement.

This comprehensive approach ensures that the `if` statement is correctly parsed and represented in the AST, accommodating both modern and legacy syntax while maintaining robust error handling.