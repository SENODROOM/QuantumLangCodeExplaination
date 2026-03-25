# `parseInputStmt` Function

The `parseInputStmt` function in the Quantum Language compiler's parser handles different forms of input statements, including both C-style and custom syntaxes. This function is crucial for parsing user input instructions within the quantum programming language.

## What It Does

This function parses an input statement and constructs an abstract syntax tree (AST) node representing that statement. The supported forms of input statements include:

1. **C-style Input**: `scanf("%d", &var)`
2. **Custom Prompted Input**: `input("prompt", var)`
3. **Custom Unprompted Input**: `input(var)`

Upon successful parsing, the function returns an `ASTNodePtr` containing an `InputStmt` object, which encapsulates the parsed information about the input statement.

## Why It Works This Way

### Handling Different Syntaxes

- **C-style Input**: The function first checks if the next token is a left parenthesis (`(`). If so, it expects a string literal followed by a comma and then an identifier. The string literal represents the format specifier, and the identifier represents the variable to store the input.
  
- **Custom Prompted Input**: Similar to C-style input, but with an additional step where the function checks for a comma before expecting an identifier. If a comma is found, it assumes there is a prompt string before the variable.

- **Custom Unprompted Input**: If no parentheses are present, the function directly expects an identifier, assuming no prompt is needed.

### Edge Cases

- **Missing Parentheses**: If the input statement starts with a left parenthesis but lacks a corresponding right parenthesis, the function will raise an error.
- **Invalid Format Specifier**: If a format specifier is provided in a C-style input but is not recognized as a valid string literal, the function will raise an error.
- **Non-Identifier Target**: If the target specified in any form of input statement is not a valid identifier, the function will raise an error.

### Interactions With Other Components

- **Token Stream Consumption**: The function consumes tokens from the token stream using methods like `consume()`, `expect()`, and `match()`. These methods ensure that the correct sequence of tokens is processed according to the grammar rules.
- **Error Reporting**: If any unexpected token or missing element is encountered during parsing, the function raises appropriate errors using methods like `error()`.
- **Abstract Syntax Tree Construction**: The parsed information is used to construct an `InputStmt` object, which is then wrapped in an `ASTNodePtr` and returned. This AST node can be further processed by other parts of the compiler to generate executable code.

## Parameters/Return Value

### Parameters

- None explicitly listed in the function signature, but it relies on global state managed by the parser, such as the current token and the token stream.

### Return Value

- Returns an `ASTNodePtr` pointing to an `InputStmt` object, which contains the parsed information about the input statement.
- Each `InputStmt` includes:
  - A `std::string` representing the target variable.
  - An `ASTNodePtr` representing the prompt (if applicable).

## Example Usage

Here’s how you might use this function in the context of parsing a quantum program:

```cpp
// Assuming 'parser' is an instance of the Parser class
auto stmt = parser.parseInputStmt();

// Now 'stmt' is an ASTNodePtr containing an InputStmt object
// You can further process this AST node to generate executable code
```

In summary, the `parseInputStmt` function is designed to handle various forms of input statements in the Quantum Language, ensuring robust parsing and construction of AST nodes for subsequent compilation steps.