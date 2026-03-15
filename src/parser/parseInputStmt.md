# `parseInputStmt`

The `parseInputStmt` function in the Quantum Language compiler is designed to handle different types of input statements. These include C-style input using `scanf`, and custom input functions `input` with optional prompts. The function parses these statements and constructs an abstract syntax tree (AST) node representing the input operation.

## What It Does

The primary purpose of `parseInputStmt` is to parse input statements and convert them into AST nodes that can be further processed by the compiler. This involves recognizing various forms of input syntax and extracting necessary information such as the format string, prompt, and target variable.

## Why It Works This Way

1. **Flexibility**: By handling both C-style and custom input functions, the function provides flexibility in how input operations are defined within the Quantum Language.
2. **Error Handling**: The function includes error handling mechanisms to ensure that the input statement is correctly formatted. If any expected token is missing, it throws an exception with a descriptive message.
3. **Abstract Syntax Tree Construction**: The parsed input statement is converted into an AST node (`InputStmt`) which encapsulates the target variable and the prompt (if present). This allows for easier manipulation and code generation during subsequent compilation phases.

## Parameters/Return Value

### Parameters

- None explicitly listed in the provided code snippet. However, it relies on global state managed by the parser, such as the current token and its type.

### Return Value

- Returns a unique pointer to an `ASTNode` object representing the parsed input statement.
- If the input statement is invalid, it throws an exception.

## Edge Cases

1. **C-style Input**: Handles `scanf("%d", &var)` where `%d` is a format specifier and `&var` is the address-of operator pointing to a target variable.
2. **Custom Input with Prompt**: Handles `input("prompt", var)` where `"prompt"` is a string literal and `var` is the target variable.
3. **Custom Input Without Prompt**: Handles `input(var)` where `var` is the target variable without a prompt.
4. **Optional Address-of Operator**: Supports both `input(&var)` and `input(var)` syntaxes, treating them equally.
5. **Whitespace and Semicolons**: Ignores trailing whitespace and semicolons at the end of the input statement.

## Interactions With Other Components

- **Token Stream Management**: Relies on the global token stream managed by the parser to fetch and consume tokens.
- **Error Reporting**: Utilizes error reporting mechanisms to provide feedback if the input statement is malformed.
- **AST Node Creation**: Creates instances of `ASTNode` objects to represent different parts of the input statement, such as `StringLiteral` for the prompt and `InputStmt` for the entire input operation.
- **Compilation Context**: May interact with other components of the compiler to generate appropriate machine code or intermediate representation based on the parsed AST.

This comprehensive approach ensures that the `parseInputStmt` function can effectively handle various input statement formats and construct accurate AST nodes for further processing.