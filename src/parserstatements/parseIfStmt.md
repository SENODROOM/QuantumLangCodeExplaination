# `parseIfStmt` Function

## Purpose
The `parseIfStmt` function in the Quantum Language compiler is designed to parse an `if` statement from the token stream and construct the corresponding Abstract Syntax Tree (AST). It supports both traditional `if` statements and the more modern `if-with-initializer` syntax introduced in C++17.

## Parameters
- **None**: The function operates directly on the global token stream (`tokens`) and its position (`pos`). It does not take any explicit parameters.

## Return Value
- **ASTNodePtr**: A pointer to the root of the constructed AST representing the parsed `if` statement.

## How It Works
The function begins by consuming the `if` keyword and checking the subsequent tokens to determine whether it's parsing a traditional `if` statement or an `if-with-initializer` statement.

### Traditional `if` Statement
For a traditional `if` statement, the function expects the following structure:
```cpp
if (condition) { ... }
```
It consumes the opening parenthesis `(`, parses the condition expression using `parseExpr()`, checks for a closing parenthesis `)` using `expect()`, matches the colon `:` using `match()`, skips any new lines using `skipNewlines()`, and finally parses the body of the `if` statement using `parseBodyOrStatement()`.

### `if-with-initializer` Statement
For an `if-with-initializer` statement, which follows the C++17 standard, the function expects the following structure:
```cpp
if (auto varName = initializer; condition) { ... }
```
Here's how the function handles this case:
1. Consumes the opening parenthesis `(`.
2. Checks if the next token is `auto`.
3. Consumes `auto` and then the variable name using `consume()`. This variable name will be used as the identifier in the condition.
4. Consumes the assignment operator `=`.
5. Parses the initializer expression using `parseExpr()`.
6. Consumes the closing parenthesis `)` using `expect()`.
7. Matches the colon `:` using `match()`.
8. Skips any new lines using `skipNewlines()`.
9. Parses the body of the `if` statement using `parseBodyOrStatement()`.

After constructing the AST nodes for the variable declaration and the condition, the function creates an `IfStmt` node that encapsulates these elements along with the parsed `then` block.

### Handling `else` Branch
The function also handles optional `else` branches:
- If the next token is `else`, it consumes `else` and then checks if the following token is `if`. If so, it recursively calls `parseIfStmt()` to handle nested `if` statements within the `else` branch.
- If the `else` branch is not a nested `if`, it matches the colon `:` and then parses the `else` block using `parseBodyOrStatement()`.

Finally, the function constructs the complete AST by combining the variable declaration, condition, `then` block, and `else` block into an `IfStmt` node.

## Edge Cases
- **Missing Parentheses**: The function ensures that the opening and closing parentheses are present before proceeding with the rest of the parsing steps. Missing parentheses result in errors.
- **Incorrect Assignment Operator**: For `if-with-initializer` statements, the function specifically looks for the assignment operator `=` rather than the equality operator `==`. Using `==` instead of `=` would lead to a syntax error.
- **Nested `if` Statements**: The function correctly handles `else` branches that contain another `if` statement, ensuring proper nesting and parsing.

## Interactions With Other Components
- **Token Stream Consumption**: The function uses methods like `consume()`, `expect()`, and `match()` to advance through the token stream and validate the syntax.
- **Expression Parsing**: The `parseExpr()` method is called to parse the conditional expression and the initializer expression.
- **Body Parsing**: The `parseBodyOrStatement()` method is used to parse the code block associated with the `if` statement.
- **Error Handling**: Various methods like `expect()` and `match()` help in identifying and reporting syntax errors.

This function plays a crucial role in parsing complex control flow structures in the Quantum Language compiler, ensuring that the resulting AST accurately reflects the source code's intent.