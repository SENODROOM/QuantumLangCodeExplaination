# `parseInputStmt`

The `parseInputStmt` function in the Quantum Language compiler is responsible for parsing various forms of input statements encountered in the source code. These include both C-style input using `scanf` and custom input functions `input` with optional prompts. The function processes these statements and constructs an abstract syntax tree (AST) node representing the parsed input statement.

## What It Does

The primary role of `parseInputStmt` is to correctly interpret and convert input statements into their corresponding AST nodes. This allows the compiler to understand how variables should be populated with user input during execution.

## Why It Works This Way

The function's design ensures flexibility and robustness in handling different types of input statements. By checking the token type and consuming tokens accordingly, it can differentiate between C-style `scanf` and custom `input` functions, including those with prompts. This approach allows the function to adapt to various syntactic structures without requiring significant changes.

## Parameters/Return Value

- **Parameters**: None explicitly declared within the function signature; however, it relies on global state managed by the parser, such as the current token (`current()`) and methods like `consume()` and `expect()`.
  
- **Return Value**: Returns a unique pointer to an `ASTNode` containing an `InputStmt`. The `InputStmt` includes the target variable and an optional prompt.

## Edge Cases

1. **C-style Input**: The function handles C-style input using `scanf` where the first argument is a format string followed by a comma and the address-of operator (`&`). If the address-of operator is omitted, it assumes standard `scanf` usage.
   
2. **Custom Input Function**: For custom `input` functions, the function can accept either a single identifier (no prompt) or a pair consisting of a string literal (prompt) and an identifier (target variable). If the prompt is missing, it defaults to an empty string.

3. **Whitespace Handling**: The function consumes any newline characters (`NEWLINE`) or semicolons (`SEMICOLON`) that may follow the input statement, ensuring proper parsing even when statements are separated by whitespace.

4. **Error Handling**: In cases where expected tokens are not found, the function throws errors indicating the nature of the issue (e.g., "Expected variable name after ','").

## Interactions With Other Components

- **Token Stream Management**: The function interacts with the token stream provided by the parser. It uses methods like `current()`, `consume()`, and `expect()` to navigate through the tokens and validate their correctness.
  
- **Abstract Syntax Tree Construction**: After parsing the input statement, the function constructs an `ASTNode` representing the parsed statement. This node is then used by subsequent stages of the compilation process to generate executable code.

- **Error Reporting**: If the input statement is malformed, the function reports appropriate error messages, facilitating debugging and correction of the source code.

Overall, `parseInputStmt` plays a crucial role in accurately interpreting input statements in the Quantum Language, ensuring that the resulting AST reflects the intended behavior of the program.