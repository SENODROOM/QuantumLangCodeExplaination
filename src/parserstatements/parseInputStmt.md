# `parseInputStmt` Function

The `parseInputStmt` function in the Quantum Language compiler's parser is designed to handle different forms of input statements, including both C-style and custom syntaxes. This function is crucial for parsing user input instructions within the quantum programming language, ensuring that the correct syntax is used and that the input is properly assigned to variables.

## What It Does

The primary purpose of the `parseInputStmt` function is to parse an input statement and construct an abstract syntax tree (AST) node representing that statement. The function supports three types of input statements:

1. **C-style Input**: This follows the format `scanf("%d", &var);`, where `%d` is a format specifier and `&var` is the address of the target variable.
2. **Custom Syntax with Prompt**: This follows the format `input("prompt", var);`, where `"prompt"` is the message displayed to the user, and `var` is the target variable.
3. **Custom Syntax without Prompt**: This follows the format `input(var);`, where `var` is the target variable, and no prompt is provided.

## Why It Works This Way

The function works by examining the token stream to determine the type of input statement being parsed. It uses conditional checks (`if` and `else`) to identify whether the statement is C-style, custom with a prompt, or custom without a prompt. Depending on the identified type, the function constructs the appropriate AST node using `std::make_unique`.

- For C-style input, the function expects a format string followed by a comma and the target variable. If the format string is not followed by a comma, it assumes a custom syntax without a prompt.
- For custom syntax, the function expects a string literal followed by an optional comma and the target variable. If the comma is present, it further expects the target variable to be preceded by the `&` operator.

This approach ensures that the function can accurately parse different forms of input statements and construct the corresponding AST nodes.

## Parameters/Return Value

### Parameters

- None

### Return Value

- `ASTNodePtr`: A pointer to an AST node representing the parsed input statement. The returned node contains information about the target variable and any associated prompt.

## Edge Cases

1. **Missing Comma**: If the C-style input statement is missing a comma between the format string and the target variable, the function will assume a custom syntax without a prompt.
2. **Incorrect Token Types**: If the expected token types are not found in the token stream, such as a missing closing parenthesis or an incorrect identifier, the function will raise an error.
3. **Empty Prompt**: In the case of custom syntax without a prompt, the function returns an empty string for the prompt field.

## Interactions With Other Components

The `parseInputStmt` function interacts closely with the tokenizer component to retrieve tokens from the source code. It also relies on the `expect` and `consume` functions to ensure that the correct sequence of tokens is encountered and consumed during parsing.

Upon successful parsing, the constructed AST node is passed to the semantic analysis phase, which further validates the input statement and ensures that the target variable exists and is properly declared. Additionally, the AST node may be used by the code generation phase to produce the appropriate machine code for executing the input statement.