# `parseInputStmt` Function

The `parseInputStmt` function in the Quantum Language compiler's parser is responsible for handling various forms of input statements used in quantum programming. These forms include both C-style input using format strings and custom syntaxes such as prompts followed by target variables. The primary goal of this function is to correctly parse these input statements into an Abstract Syntax Tree (AST), which can then be further processed by the compiler.

## What It Does

The `parseInputStmt` function parses input statements based on their structure and constructs an appropriate AST node representing the input operation. It supports three main types of input statements:

1. **C-style Input**: `scanf("%d", &var)`
2. **Prompted Input**: `input("prompt", var)`
3. **Simple Input**: `input(var)`

For each type, the function extracts the necessary information (such as the format string, prompt message, and target variable) and creates an `InputStmt` AST node containing this data.

## Why It Works This Way

The function works in this manner because it needs to accommodate different syntaxes that users might employ when writing quantum programs. By breaking down the parsing logic into conditional checks and consuming tokens accordingly, the function ensures that it can handle all valid input statement formats.

- **C-style Input Handling**: When encountering a format string (`TokenType::STRING`) followed by a comma and a target variable (`TokenType::IDENTIFIER`), the function recognizes this as a C-style input and constructs an `InputStmt` node with the provided format string and target variable.
  
- **Prompted Input Handling**: If the format string is followed by a comma but no target variable, the function assumes this is a prompted input and constructs an `InputStmt` node with the provided prompt message and an empty target variable.
  
- **Simple Input Handling**: For simple input without any additional arguments, the function directly consumes the target variable and constructs an `InputStmt` node with an empty prompt message.

## Parameters/Return Value

### Parameters

- None

### Return Value

- An `ASTNodePtr` pointing to an `InputStmt` node. This node contains the parsed input statement details, including the target variable and any associated prompt message.

## Edge Cases

1. **Missing Comma**: If a C-style input statement is missing the comma between the format string and the target variable, the function will throw an error indicating "Expected variable name after ','".
   
2. **Extra Characters**: Any unexpected characters or misplaced tokens within the input statement will result in errors like "Expected ')'".

3. **Empty Target Variable**: In cases where the input statement specifies a prompt but no target variable (e.g., `input("prompt")`), the function sets the target variable to an empty string.

4. **Multiple Statements**: If multiple input statements are separated by newlines or semicolons, the function skips over these separators before returning the parsed `InputStmt` node.

## Interactions With Other Components

The `parseInputStmt` function interacts closely with other components of the parser, such as token checking and consumption functions (`check`, `consume`, `expect`). These helper functions enable the `parseInputStmt` to accurately identify and process the tokens forming the input statement.

Additionally, the function uses the `current()` method to retrieve the current token being processed and the `ln` variable to store the line number of the input statement. This information is crucial for constructing the AST node with accurate context.

Overall, the `parseInputStmt` function plays a vital role in ensuring that input statements in the Quantum Language are correctly parsed and represented in the AST, facilitating subsequent compilation steps.