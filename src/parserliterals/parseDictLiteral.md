# `parseDictLiteral` Function

## Overview

The `parseDictLiteral` function in the Quantum Language compiler is responsible for parsing dictionary literals from the source code. Dictionary literals are represented using curly braces `{}`, and they consist of key-value pairs. This function ensures that the syntax of dictionary literals is correctly parsed and constructs an appropriate Abstract Syntax Tree (AST) node for them.

### Why It Works This Way

The function works by iterating through the tokens in the source code and constructing the dictionary literal step-by-step. It handles both standard key-value pairs and shorthand properties where the key is implied by the value. The function also supports spread syntax, allowing dictionary literals to be constructed from existing objects.

### Parameters

- **None**: The function operates directly on the global state of the parser, accessing the current token position and modifying it as necessary.

### Return Value

- **`DictLiteral`**: An AST node representing the parsed dictionary literal. If there is an error during parsing, the function will throw an exception.

### Edge Cases

1. **Empty Dictionary Literal**: If the dictionary literal is empty (`{}`), the function should return an empty `DictLiteral`.
2. **Spread Syntax**: If the dictionary literal contains spread syntax (`{...obj, key: val}`), the function should handle it appropriately and construct the AST node accordingly.
3. **Syntax Errors**: If the dictionary literal has incorrect syntax (e.g., missing colon, extra comma), the function should throw an appropriate exception.

### Interactions With Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens for parsing.
- **Error Handling**: The function uses error handling mechanisms provided by the compiler to report syntax errors.
- **AST Construction**: The function constructs AST nodes for the keys and values of the dictionary literal, which are then used to build the final `DictLiteral` node.

Here is a more detailed breakdown of the function:

```cpp
int ln = current().line; // Get the current line number for error reporting
expect(TokenType::LBRACE, "Expected '{'"); // Ensure the first token is an opening brace
skipNewlines(); // Skip any newlines before starting the parsing loop

DictLiteral dict; // Initialize the dictionary literal AST node

while (!check(TokenType::RBRACE) && !atEnd()) // Loop until we encounter a closing brace or reach the end of the input
{
    // Handle spread syntax: {...obj, key: val}
    if (check(TokenType::IDENTIFIER) && current().value == "...") 
    {
        consume(); // Consume the "..." token
        auto spreadExpr = parseUnary(); // Parse the expression following the spread operator
        // Use a special sentinel: key = nullptr means spread
        dict.pairs.emplace_back(nullptr, std::move(spreadExpr)); 
        skipNewlines(); // Skip any newlines after the spread expression
        if (!match(TokenType::COMMA)) // If the next token is not a comma, exit the loop
            break;
        skipNewlines(); // Skip any newlines after the comma
        if (check(TokenType::RBRACE)) // If the next token is a closing brace, exit the loop
            break;
        continue;
    }

    // Handle key-value pairs
    ASTNodePtr key;
    bool isShorthand = false;

    if (check(TokenType::IDENTIFIER) || isCTypeKeyword(current().type) || check(TokenType::TYPE_STRING))
    {
        // Peek ahead to determine if the next token is a colon
        size_t la = pos + 1;
        while (la < tokens.size() && tokens[la].type == TokenType::NEWLINE)
            la++;

        if (la < tokens.size() && tokens[la].type == TokenType::COLON)
        {
            // Bare identifier key: firstName → StringLiteral "firstName"
            auto keyName = consume().value;
            key = std::make_unique<ASTNode>(StringLiteral{keyName}, ln);
        }
        else if (la < tokens.size() && (tokens[la].type == TokenType::COMMA || tokens[la].type == TokenType::RBRACE))
        {
            // Shorthand property: { x } or { x, y }
            auto keyName = consume().value;
            key = std::make_unique<ASTNode>(StringLiteral{keyName}, ln);
            isShorthand = true;
        }
        else
            key = parseExpression(); // Parse the key expression
    }

    // Expect a colon after the key
    expect(TokenType::COLON, "Expected ':'");

    // Parse the value expression
    ASTNodePtr value = parseExpression();

    // Add the key-value pair to the dictionary literal
    dict.pairs.emplace_back(std::move(key), std::move(value));

    // Skip any newlines after the value
    skipNewlines();

    // Check for trailing commas or closing braces
    if (!match(TokenType::COMMA))
        break;
    skipNewlines();
    if (check(TokenType::RBRACE))
        break;
}

// Ensure the last token is a closing brace
expect(TokenType::RBRACE, "Expected '}'");

return dict; // Return the constructed dictionary literal AST node
```

This function provides a robust mechanism for parsing dictionary literals in the Quantum Language compiler, ensuring that all valid syntax is handled correctly and that appropriate error messages are reported for invalid inputs.