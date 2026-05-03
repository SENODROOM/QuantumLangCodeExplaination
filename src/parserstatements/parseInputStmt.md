# `parseInputStmt` Function

The `parseInputStmt` function in the Quantum Language compiler's parser is responsible for handling various forms of input statements, including both C-style and custom syntaxes. This function plays a critical role in parsing user input instructions within the quantum programming language.

## What It Does

The `parseInputStmt` function parses input statements based on the following patterns:

1. **C-style Input**: 
   - Format string followed by an address-of operator (`&`) and a target variable.
   - Example: `scanf("%d", &var);`
   
2. **Custom Syntax with Prompt**:
   - A prompt string followed by a comma and a target variable.
   - Example: `input("Enter a number:", var);`
   
3. **Custom Syntax without Prompt**:
   - Only a target variable.
   - Example: `input(var);`

After parsing the input statement, it constructs an `ASTNode` representing the parsed input instruction and returns it.

## Why It Works This Way

The function works as described because it needs to handle multiple syntax patterns for input statements. By checking token types and consuming tokens accordingly, it can differentiate between the different patterns and correctly parse each one into its constituent parts.

- **C-style Input**: The function first checks if the next token is a left parenthesis (`(`). If so, it consumes it and then checks if the next token is a string literal. If it is, the function assumes that the string is a format string and expects a comma followed by an identifier (the target variable). If the comma is not present, it assumes that the string is a prompt and leaves the target variable empty. After parsing the format string or prompt, the function expects a right parenthesis (`)`).
  
- **Custom Syntax with Prompt**: Similar to C-style input, but instead of a format string, the function expects a string literal as the prompt. After parsing the prompt, it expects a comma followed by an identifier (the target variable).

- **Custom Syntax without Prompt**: If the next token is an identifier directly after the left parenthesis, the function assumes that there is no prompt and expects a right parenthesis (`)`).

Once the input statement is fully parsed, the function skips any trailing newlines or semicolons and returns an `ASTNode` containing the parsed input instruction.

## Parameters/Return Value

### Parameters

- None

### Return Value

- Returns a unique pointer to an `ASTNode` representing the parsed input instruction.

## Edge Cases

- **Empty Target Variable**: When parsing the custom syntax without a prompt, the target variable is set to an empty string.
- **Missing Tokens**: The function includes error handling to ensure that required tokens (e.g., parentheses, commas, identifiers) are present. If any required token is missing, an appropriate error message is generated.
- **Invalid Token Types**: The function checks for valid token types throughout the parsing process. If an unexpected token type is encountered, an error message is generated.

## Interactions With Other Components

The `parseInputStmt` function interacts with several other components of the Quantum Language compiler's parser:

- **Token Stream Management**: The function uses the token stream provided by the parser to read and consume tokens.
- **Error Handling**: The function includes error handling mechanisms to manage cases where expected tokens are missing or invalid.
- **Abstract Syntax Tree Construction**: The function constructs an `ASTNode` representing the parsed input instruction, which is used by other components of the compiler for further processing.

In summary, the `parseInputStmt` function is designed to handle various forms of input statements in the Quantum Language compiler, ensuring correct parsing and construction of the abstract syntax tree.