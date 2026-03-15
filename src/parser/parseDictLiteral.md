# `parseDictLiteral`

## Overview

The `parseDictLiteral` function in the Quantum Language compiler is responsible for parsing dictionary literals from the source code. Dictionary literals are represented as `{}` and can contain pairs of keys and values, as well as shorthand properties.

## Functionality

### What It Does

- **Parse Dictionary Literals**: The function reads the source code to construct an abstract syntax tree (AST) node representing a dictionary literal.
- **Handle Spread Syntax**: It supports the spread syntax (`...`) within dictionary literals, allowing for the merging of another dictionary into the current one.
- **Support Various Key Types**: Keys in dictionary literals can be strings, numbers, identifiers, or type keywords.

### Why It Works This Way

- **Flexibility**: By supporting various types of keys, the function provides flexibility in how dictionaries can be defined and used in the language.
- **Spread Syntax**: The spread syntax allows for dynamic merging of dictionaries, which is useful for creating complex data structures without manually copying each key-value pair.
- **Edge Case Handling**: The function includes checks for different token types and positions to handle edge cases such as empty dictionaries, trailing commas, and incorrect syntax gracefully.

## Parameters and Return Value

### Parameters

- None explicitly listed in the provided snippet, but implied parameters include:
  - `current()`: A reference to the current token being processed.
  - `pos`: An iterator or index indicating the current position in the token stream.
  - `tokens`: A vector or list of tokens representing the source code.

### Return Value

- Returns an `ASTNodePtr`, which is a smart pointer to an `ASTNode` object representing the parsed dictionary literal.

## Edge Cases

- **Empty Dictionary**: If the dictionary literal is empty (`{}`), the function will correctly parse it without any key-value pairs.
- **Trailing Comma**: The function handles trailing commas by breaking out of the loop when encountering a comma followed by either a closing brace (`}`) or the end of the input.
- **Incorrect Syntax**: If the syntax is incorrect (e.g., missing a colon between a key and its value), the function will throw an error or handle it appropriately based on the context.

## Interactions With Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the sequence of tokens that represent the source code.
- **Error Handling**: It interacts with the error handling component to report syntax errors if encountered during parsing.
- **AST Construction**: The function constructs an AST node using the parsed information, which is then used by other parts of the compiler for further processing.

## Detailed Code Explanation

```cpp
int ln = current().line; // Get the line number of the current token
expect(TokenType::LBRACE, "Expected '{'"); // Ensure the current token is an opening brace
skipNewlines(); // Skip any newline characters before proceeding

DictLiteral dict; // Initialize the dictionary literal node

while (!check(TokenType::RBRACE) && !atEnd()) // Continue until we encounter a closing brace or reach the end of the input
{
    // Handle spread syntax: {...obj, key: val}
    if (check(TokenType::IDENTIFIER) && current().value == "...") 
    {
        consume(); // Eat the "..." token
        auto spreadExpr = parseUnary(); // Parse the expression following the spread syntax
        // Use a special sentinel: key = nullptr means spread
        dict.pairs.emplace_back(nullptr, std::move(spreadExpr)); 
        skipNewlines(); // Skip any newline characters after the spread expression
        if (!match(TokenType::COMMA)) // If the next token is not a comma, break out of the loop
            break;
        skipNewlines(); // Skip any newline characters before continuing
        if (check(TokenType::RBRACE)) // If the next token is a closing brace, break out of the loop
            break;
        continue;
    }

    // Handle key: accept quoted string, number, bare identifier, or type keyword
    ASTNodePtr key;
    bool isShorthand = false;

    if (check(TokenType::IDENTIFIER) || isCTypeKeyword(current().type) || check(TokenType::TYPE_STRING)) 
    {
        // Peek ahead — if next token after this is COLON, treat as bare string key
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
            key = parseExp(); // Parse the key expression
    }

    // Parse the value associated with the key
    ASTNodePtr value = parseExp();

    // Add the key-value pair to the dictionary
    dict.pairs.emplace_back(std::move(key), std::move(value));

    // Skip any newline characters after the key-value pair
    skipNewlines();

    // Check for the next token
    if (!match(TokenType::COMMA)) // If the next token is not a comma, break out of the loop
        break;
    skipNewlines