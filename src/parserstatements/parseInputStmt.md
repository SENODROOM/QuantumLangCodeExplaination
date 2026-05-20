# `parseInputStmt` Function

The `parseInputStmt` function in the Quantum Language compiler's parser is designed to handle different types of input statements encountered during the parsing process. These statements can be in the form of C-style input using format strings, or they can be custom syntaxes involving prompts and target variables. This function ensures that the input statements are correctly parsed into an abstract syntax tree (AST) node representing an input statement.

## What It Does

The primary role of the `parseInputStmt` function is to parse input statements and construct corresponding AST nodes. The function supports three main forms of input statements:

1. **C-style Input**: Using format strings similar to those in the C language.
2. **Custom Syntax with Prompt**: A prompt followed by a target variable where data will be stored.
3. **Custom Syntax without Prompt**: Only a target variable where data will be stored.

## Why It Works This Way

### Handling Different Input Formats

- **C-style Input (`scanf("%d", &var)`)**: When encountering a format string within parentheses, the function expects a comma followed by a target variable. If the comma is present, it constructs an AST node with the format string and the target variable. If the comma is absent, it assumes the entire input statement is just a prompt.
  
- **Custom Syntax with Prompt (`input("prompt", var)`)**: Similar to C-style input, but explicitly labeled as a custom syntax. The function checks for a string literal followed by a comma and then a target variable, constructing an AST node accordingly.

- **Custom Syntax without Prompt (`input(var)`)**: In this case, the function directly parses the target variable without any additional prompt information.

### Edge Cases

- **Missing Comma**: If the expected comma is missing between the format string and the target variable, the function throws an error indicating the expected token.
  
- **Incorrect Token Types**: The function validates the token types at each step to ensure they match the expected syntax. For example, it expects a string literal when it encounters one, and a variable identifier when it needs one.

- **Unmatched Parentheses**: The function ensures that all parentheses are properly matched. If a closing parenthesis is not found, it throws an error.

## Parameters/Return Value

### Parameters

- None

### Return Value

- `std::unique_ptr<ASTNode>`: Returns a unique pointer to an ASTNode representing the parsed input statement. The node contains two elements: the target variable and an optional prompt.

## Interactions With Other Components

### Lexer

The `parseInputStmt` function relies on the lexer to tokenize the input source code. It uses the tokens produced by the lexer to determine the structure of the input statement being parsed.

### Parser State Management

The function consumes tokens based on its expectations and updates the parser state accordingly. It handles newlines and semicolons to manage the flow of statements within the source code.

### Error Handling

The function includes error handling mechanisms to catch and report issues related to incorrect syntax or missing tokens. This helps maintain robustness and clarity in the parsed output.

## Detailed Code Explanation

Here’s a breakdown of how the function operates:

```cpp
int ln = current().line; // Get the current line number for error reporting
std::string target;      // Variable to store the target variable name
ASTNodePtr prompt;       // Pointer to store the prompt AST node

if (check(TokenType::LPAREN)) // Check if the next token is '('
{
    consume(); // Consume the '(' token

    if (check(TokenType::STRING)) // Check if the next token is a string literal
    {
        // First argument is a string: either a format string or a prompt
        auto fmtTok = current(); // Store the current token (format string/prompt)
        consume(); // Consume the string literal token

        if (match(TokenType::COMMA)) // Check if the next token is ','
        {
            // scanf("%d", &var) or input("prompt", var)
            prompt = std::make_unique<ASTNode>(StringLiteral{fmtTok.value}, ln); // Construct prompt AST node
            if (check(TokenType::BIT_AND)) // Check if the next token is '&'
                consume(); // Consume the '&' token (optional)

            target = expect(TokenType::IDENTIFIER, "Expected variable name after ','").value; // Expect a variable name
        }
        else
        {
            // input("prompt") — prompt only, no target
            prompt = std::make_unique<ASTNode>(StringLiteral{fmtTok.value}, ln); // Construct prompt AST node
            target = ""; // No target variable
        }
    }
    else
    {
        // input(&var) or input(var)
        if (check(TokenType::BIT_AND)) // Check if the next token is '&'
            consume(); // Consume the '&' token (optional)

        target = expect(TokenType::IDENTIFIER, "Expected variable name").value; // Expect a variable name
    }

    expect(TokenType::RPAREN, "Expected ')'"); // Ensure the ')' token is present
}

// Skip any trailing newlines or semicolons
while (check(TokenType::NEWLINE) || check(TokenType::SEMICOLON))
    consume();

// Return the constructed ASTNode representing the input statement
return std::make_unique<ASTNode>(InputStmt{target, std::move(prompt)}, ln);
```

This function effectively parses the input statements, ensuring that they conform to the specified syntax rules. By constructing appropriate AST nodes, it facilitates further processing and analysis within the compiler.