# `parseArrayLiteral` Function

## Overview

The `parseArrayLiteral` function is a key part of the Quantum Language Compiler's parser module, responsible for interpreting and converting array literals from the source code into Abstract Syntax Tree (AST) nodes. This function efficiently handles both regular array literals and list comprehensions.

### Why It Works This Way

The function uses a combination of lexical analysis and recursive descent parsing to determine whether an array literal is regular or a list comprehension. By checking token types and consuming tokens accordingly, it can accurately parse complex structures like list comprehensions without ambiguity.

## Parameters

- None

## Return Value

- An `ASTNodePtr`, which is a unique pointer to an `ASTNode`. The node represents either a regular array literal or a list comprehension.

## Edge Cases

1. **Empty Array**: If the array literal is empty (`[]`), the function returns an `ASTNode` representing an empty array literal.
2. **Single Element**: If the array contains only one element, the function parses that element and constructs a regular array literal.
3. **List Comprehension**: If the array literal includes a list comprehension syntax (`[expr for var in iterable]` or `[expr for var in iterable if cond]`), the function parses the comprehension and constructs an appropriate AST node.

## Interactions with Other Components

- **Tokenizer**: The function relies on the tokenizer to provide the next token during parsing.
- **Error Handling**: In case of unexpected tokens or syntax errors, the function throws a `ParseError`.
- **Expression Parsing**: The function calls `parseExpr()` to parse individual expressions within the array literal.

Here is a more detailed breakdown of the implementation:

```cpp
int ln = current().line; // Get the current line number
expect(TokenType::LBRACKET, "Expected '['"); // Ensure the start of the array literal is marked by '['
skipNewlines(); // Skip any new lines after the opening bracket

// Handle empty array
if (check(TokenType::RBRACKET)) {
    consume(); // Consume the closing bracket ']'
    return std::make_unique<ASTNode>(ArrayLiteral{}, ln); // Return an ASTNode for an empty array literal
}

// Parse the first expression in the array literal
auto firstExpr = parseExpr();
skipNewlines(); // Skip any new lines after the first expression

// Check for list comprehension syntax
if (check(TokenType::FOR)) {
    consume(); // Consume the 'for' keyword
    std::vector<std::string> vars; // Vector to store loop variables
    auto readVar = [&]() { // Lambda function to read loop variables
        if (check(TokenType::IDENTIFIER))
            vars.push_back(consume().value); // Consume identifier as loop variable
        else if (isCTypeKeyword(current().type))
            vars.push_back(consume().value); // Consume C type keyword as loop variable
        else
            vars.push_back(expect(TokenType::IDENTIFIER, "Expected variable in comprehension").value); // Expect an identifier
    };
    readVar(); // Read the first loop variable
    while (match(TokenType::COMMA)) // Continue reading loop variables separated by commas
        readVar();

    if (!match(TokenType::IN) && !match(TokenType::OF)) // Ensure 'in' or 'of' follows the loop variables
        throw ParseError("Expected 'in' in list comprehension", current().line, current().col); // Throw error if not found

    auto iterable = parseExpr(); // Parse the iterable expression
    skipNewlines(); // Skip any new lines after the iterable

    // Optional filter condition
    ASTNodePtr condition;
    if (check(TokenType::IF)) {
        consume(); // Consume the 'if' keyword
        condition = parseExpr(); // Parse the condition expression
        skipNewlines(); // Skip any new lines after the condition
    }

    expect(TokenType::RBRACKET, "Expected ']'"); // Ensure the end of the array literal is marked by ']'

    // Construct list comprehension node
    ListComp lc;
    lc.expr = std::move(firstExpr); // Move the first expression into the list comprehension
    lc.vars = std::move(vars); // Move the loop variables into the list comprehension
    lc.iterable = std::move(iterable); // Move the iterable into the list comprehension
    lc.condition = std::move(condition); // Move the optional condition into the list comprehension

    return std::make_unique<ASTNode>(std::move(lc), ln); // Return the constructed ASTNode for the list comprehension
} else {
    // Regular array literal
    ArrayLiteral arr;
    arr.elements.push_back(std::move(firstExpr)); // Add the first parsed expression to the array elements
    skipNewlines(); // Skip any new lines after the first element

    // Continue parsing additional elements separated by commas
    while (match(TokenType::COMMA)) {
        arr.elements.push_back(parseExpr()); // Add each subsequent parsed expression to the array elements
        skipNewlines(); // Skip any new lines after each element
    }

    expect(TokenType::RBRACKET, "Expected ']'"); // Ensure the end of the array literal is marked by ']'

    return std::make_unique<ASTNode>(arr, ln); // Return the constructed ASTNode for the regular array literal
}
```

This implementation ensures that the parser correctly identifies and handles different types of array literals, providing robust support for both simple arrays and complex list comprehensions.