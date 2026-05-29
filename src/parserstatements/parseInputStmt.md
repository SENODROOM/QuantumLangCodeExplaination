# `parseInputStmt` Function

The `parseInputStmt` function in the Quantum Language compiler's parser is designed to handle different types of input statements encountered during the parsing process. These statements can be either C-style input using format strings or custom syntax that includes prompts for user input.

## What It Does

The primary role of `parseInputStmt` is to parse input statements and construct an Abstract Syntax Tree (AST) node representing these statements. The function supports three forms of input statements:

1. **C-style Input**: This form uses a format string followed by a target variable. For example, `scanf("%d", &var);`.
2. **Custom Prompt Input**: This form includes a prompt message followed by a target variable. For example, `input("Enter a number:", var);`.
3. **Variable-only Input**: This form simply takes a target variable without any additional information. For example, `input(var);`.

## Why It Works This Way

The function works by first identifying the type of input statement based on the tokens it encounters. If the token sequence starts with a left parenthesis (`(`), it indicates a C-style input or a custom prompt input. If the next token is a string literal, it is assumed to be a format string or a prompt message. Depending on whether a comma is found, the function determines whether there is a target variable specified. If a comma is present, it expects another identifier as the target variable. If no comma is present, it assumes that the entire statement is a prompt-only input. After parsing the input statement, the function consumes any trailing newline characters or semicolons before returning the constructed AST node.

## Parameters/Return Value

- **Parameters**:
  - None explicitly declared, but relies on global state managed by the parser (e.g., `current()`, `consume()`).

- **Return Value**:
  - Returns a unique pointer to an `ASTNode` object containing an `InputStmt`. The `InputStmt` encapsulates the target variable and the optional prompt message.

## Edge Cases

- **Empty Target Variable**: When encountering a `input("prompt");` statement, the function sets the target variable to an empty string.
- **Missing Tokens**: If the expected tokens (e.g., string literal, identifier, parentheses) are not found, the function throws an error indicating the missing element.
- **Trailing Characters**: Any trailing newline characters or semicolons are consumed to ensure proper parsing of subsequent statements.

## Interactions With Other Components

- **Token Stream Management**: The function interacts with the token stream managed by the parser to identify and consume tokens.
- **Error Handling**: The function includes error handling mechanisms to manage unexpected token sequences and provide meaningful error messages.
- **AST Construction**: The parsed input statements are used to construct an AST, which is essential for further compilation steps such as semantic analysis and code generation.

This comprehensive approach ensures that the `parseInputStmt` function can accurately handle various input statement formats and contribute effectively to the overall parsing process of the Quantum Language compiler.