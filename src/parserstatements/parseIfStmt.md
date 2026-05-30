# `parseIfStmt` Function

## Purpose
The `parseIfStmt` function in the Quantum Language compiler is designed to parse an `if` statement from the token stream and construct the corresponding Abstract Syntax Tree (AST). It supports both traditional `if` statements and the more modern `if-with-initializer` syntax introduced in C++17.

## Parameters
- **None**: The function operates directly on the global state of the parser, which includes the current position in the token stream (`pos`) and the list of tokens (`tokens`).

## Return Value
- **ASTNodePtr**: A pointer to the root node of the constructed AST representing the parsed `if` statement.

## How It Works
The function begins by consuming the `if` keyword and checking the subsequent tokens to determine if they form an `if-with-initializer`. This involves looking ahead at the next three tokens:
- The first token should be an opening parenthesis `(`.
- The second token should be the identifier `auto`.
- The third token should be another identifier, which will be used as the variable name.
- The fourth token should be an assignment operator `=`.
- The fifth token should be the closing parenthesis `)`.

If these conditions are met, the function proceeds to parse the initializer expression using `parseExpr()` and constructs an `ASTNode` for the variable declaration. The condition expression for the `if` statement is set to the variable itself, and the body of the `if` statement is parsed using `parseBodyOrStatement()`.

If the `if` statement does not have an `else` branch, the function returns the constructed `ASTNode` representing the `if` statement.

If the `if` statement has an `else` branch, the function checks whether the `else` is followed by another `if` statement. If so, it recursively calls `parseIfStmt()` to parse the nested `if` statement. Otherwise, it parses the body or statement following the `else` keyword.

Finally, the function emits the complete `if` statement, including the variable declaration, condition expression, and bodies for both the `if` and `else` branches.

## Edge Cases
- **Missing Tokens**: If any expected token is missing from the token stream, the function will throw an error indicating the expected token.
- **Incorrect Syntax**: If the syntax of the `if` statement is incorrect, such as having mismatched parentheses or an unexpected token, the function will throw an error.
- **Nested If Statements**: The function can handle nested `if` statements within the `else` branch.

## Interactions With Other Components
- **Token Stream Consumption**: The function consumes tokens from the global token stream (`pos` and `tokens`). These tokens represent the structure of the source code being compiled.
- **Error Handling**: The function uses various helper functions like `expect()` and `match()` to ensure correct syntax and throw errors when necessary.
- **AST Construction**: The function constructs nodes in the AST using classes like `VarDecl`, `Identifier`, and `IfStmt`. These nodes represent different parts of the `if` statement, such as variable declarations and conditional expressions.
- **Whitespace Skipping**: The function skips newlines using `skipNewlines()` to ensure proper parsing of the statement structure without being affected by whitespace.

This comprehensive approach ensures that `parseIfStmt` correctly handles both traditional and modern `if` statement syntaxes while maintaining robust error handling and interaction with the rest of the compiler's components.