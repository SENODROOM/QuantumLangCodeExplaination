# `parseInputStmt` Function

The `parseInputStmt` function in the Quantum Language compiler's parser is responsible for handling various forms of input statements encountered during the parsing process. These statements include both C-style input using format strings and custom syntax for user prompts.

## What It Does

The function parses an input statement and constructs an Abstract Syntax Tree (AST) node representing that statement. The possible forms of input statements are:

1. **C-style Input**: `scanf("%d", &var)`
2. **Custom Prompt Input**: `input("prompt", var)`
3. **Simple Variable Input**: `input(var)`

For each form, the function extracts the necessary information (format string, prompt, and target variable) and creates an appropriate AST node.

## Why It Works This Way

The function works by first identifying the type of input statement based on the tokens it encounters. It then consumes the relevant tokens to extract the format string, prompt, and target variable. The use of `std::unique_ptr<ASTNode>` ensures that the memory allocated for the AST nodes is properly managed and avoids memory leaks.

### Handling C-style Input

If the function encounters a left parenthesis (`(`), it expects a string token which could represent a format string or a prompt. If a comma follows the string, it assumes the statement is in the form `scanf("%d", &var)` or `input("prompt", var)`. In this case, it constructs an AST node for the prompt and extracts the target variable. If no comma is found, it assumes the statement is in the form `input("prompt")`, where there is no target variable.

### Handling Custom Prompt Input

If the function encounters a left parenthesis but no string immediately following it, it assumes the statement is in the form `input(&var)` or `input(var)`. In this case, it checks for the optional `&` operator and then expects an identifier token representing the target variable.

### Handling Simple Variable Input

If the function does not encounter a left parenthesis at all, it assumes the statement is in the form `input(var)`. It directly expects an identifier token representing the target variable.

## Parameters/Return Value

- **Parameters**:
  - None

- **Return Value**:
  - An `ASTNodePtr` pointing to an `InputStmt` node constructed from the parsed input statement.

## Edge Cases

- **Empty Target Variable**: When encountering `input("prompt")`, the function sets the target variable to an empty string.
- **Optional `&` Operator**: The function allows for an optional `&` operator before the target variable in C-style input statements (`scanf("%d", &var)`).
- **Missing Parentheses**: If the function encounters a left parenthesis but does not find the corresponding right parenthesis (`)`), it raises an error.

## Interactions With Other Components

- **Token Stream**: The function interacts with the token stream provided by the parser to identify and consume tokens.
- **Error Handling**: The function uses the `expect` method to ensure that required tokens are present and raises errors if they are missing.
- **AST Construction**: The function constructs AST nodes using the parsed information and returns them as `ASTNodePtr`.

Overall, the `parseInputStmt` function plays a crucial role in parsing input statements and ensuring that the correct AST nodes are constructed for further processing by the compiler.