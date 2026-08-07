# `parseInputStmt` Function

The `parseInputStmt` function in the Quantum Language compiler's parser is designed to handle different types of input statements, including both C-style input using format strings and custom syntax that allows for more flexible input operations. This function is crucial for correctly interpreting user inputs within the quantum programming language context.

## What It Does

The primary role of `parseInputStmt` is to parse input statements from the source code and construct an appropriate Abstract Syntax Tree (AST) node representing these statements. The function supports three forms of input statements:

1. **C-style Input**: `scanf("%d", &var)`
2. **Prompt-based Input**: `input("prompt", var)`
3. **Variable-only Input**: `input(var)`

## Why It Works This Way

The function works by examining the tokens in the source code to determine which form of input statement is being parsed. It uses token checking and consumption to navigate through the input sequence and extract relevant information such as format strings, prompts, and target variables.

### Token Checking and Consumption

- **Token Type Check**: The function checks the type of the current token to decide how to proceed. For example, if the current token is a left parenthesis (`(`), it indicates the start of a parameterized input statement.
  
- **Consuming Tokens**: Once a token is identified, it is consumed using the `consume()` method. This advances the parser to the next token in the sequence.

### Handling Different Forms of Statements

- **C-style Input**:
  - If the first token inside the parentheses is a string literal, it is treated as a format string.
  - If a comma follows the string literal, another token is expected to be an identifier, which represents the target variable.
  - If no comma is present, the string literal is still considered the prompt, but there is no target variable.

- **Prompt-based Input**:
  - Similar to C-style input, if the first token inside the parentheses is a string literal, it is treated as the prompt.
  - An additional comma followed by an identifier specifies the target variable.

- **Variable-only Input**:
  - If the first token inside the parentheses is not a string literal, it is assumed to be the target variable.
  - No prompt is provided in this case.

## Parameters/Return Value

- **Parameters**:
  - None explicitly defined in the function signature; it relies on global state managed by the parser.

- **Return Value**:
  - Returns a unique pointer to an `ASTNode` object representing the parsed input statement. The `InputStmt` constructor takes three arguments:
    - `target`: A string representing the name of the target variable.
    - `prompt`: A unique pointer to an `ASTNode` containing the prompt text (if any).
    - `formatStr`: A unique pointer to an `ASTNode` containing the format string (for C-style input).

## Edge Cases

- **Missing Parentheses**: If the input statement lacks parentheses, the function will throw an error indicating the expected presence of `(`.
- **Incorrect Token Order**: If the tokens do not follow the expected order (e.g., a string literal without a subsequent identifier), the function will raise an error.
- **Invalid Variable Names**: If the target variable specified is not a valid identifier, the function will report an error.

## Interactions With Other Components

- **Tokenizer**: The function interacts with the tokenizer to retrieve tokens from the source code.
- **Error Handling**: It utilizes error-handling mechanisms provided by the parser to manage unexpected situations gracefully.
- **AST Construction**: The function constructs an AST based on the parsed input, which can then be used by other parts of the compiler for further processing.

Overall, `parseInputStmt` is a vital component of the Quantum Language compiler's parser, ensuring that input statements are correctly interpreted and represented in the AST. Its design flexibility accommodates both traditional C-style input and more modern, prompt-driven input methods, making it adaptable to evolving language standards and user preferences.