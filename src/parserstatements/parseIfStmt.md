# `parseIfStmt` Function

## Purpose
The `parseIfStmt` function in the Quantum Language compiler is responsible for parsing an `if` statement from the token stream and constructing the corresponding Abstract Syntax Tree (AST). This function handles both the traditional `if` statement and the modern `if-with-initializer` syntax introduced in C++17.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: An `ASTNodePtr` representing the parsed `if` statement node.

## How It Works
The function starts by consuming the `if` keyword and then checks if the next tokens form an `if-with-initializer`. If they do, it parses the initializer expression and constructs a variable declaration node. The condition of the `if` statement is then set to the identifier of the declared variable. After parsing the condition, the function proceeds to parse the body or statement associated with the `if` block. If an `else` keyword is encountered, the function checks if it is followed by another `if` keyword, indicating a nested `if-else` structure. In such cases, it recursively calls `parseIfStmt` to handle the nested `if` statement. Otherwise, it parses the `else` block similarly to the `if` block. Finally, the function emits an `IfStmt` node containing the condition, `then` block, and optional `else` branch.

## Edge Cases
- **Empty Else Block**: If the `else` block is empty, the function will still construct an `IfStmt` node without an `else` branch.
- **Nested If Statements**: The function can handle nested `if-else` structures correctly by recursively calling itself when encountering an `if` within an `else`.
- **Invalid Syntax**: If the token stream does not conform to the expected `if` statement syntax, the function will throw an error message indicating the expected token type.

## Interactions With Other Components
- **Tokenizer**: The function consumes tokens from the tokenizer to build the AST.
- **Error Handling**: The function uses error handling mechanisms provided by the compiler to report syntax errors.
- **Abstract Syntax Tree (AST)**: The function constructs nodes in the AST based on the parsed tokens, including variable declarations and conditional statements.
- **Scope Management**: Although not explicitly shown in the code snippet, the function likely interacts with scope management mechanisms to ensure that variables declared within `if-with-initializer` blocks are properly scoped and accessible within their respective blocks.