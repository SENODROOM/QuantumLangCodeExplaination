# `parseDictLiteral` Function

## Overview

The `parseDictLiteral` function in the Quantum Language compiler is responsible for parsing dictionary literals from the source code. Dictionary literals are defined using curly braces `{}`, containing key-value pairs. The function ensures accurate parsing of these structures, handling various syntax cases including shorthand properties and spread operators.

## Parameters

- **None**: The function operates directly on the global state of the parser, accessing the current token position and modifying it as necessary.

## Return Value

- **`DictLiteral`**: A structure representing the parsed dictionary literal. It contains a vector of pairs, where each pair consists of an optional key and a corresponding value expression.

## Edge Cases

- **Empty Dictionary**: If the dictionary is empty (`{}`), the function will return an empty `DictLiteral`.
- **Invalid Syntax**: The function checks for proper syntax, such as missing closing braces (`}`) or misplaced commas. If invalid syntax is detected, it throws an error.
- **Spread Operator**: The function supports the spread operator (`...`) to allow merging of dictionaries. For example, `{ ...obj, key: val }`.

## Interactions with Other Components

- **Token Stream**: The function interacts with the token stream managed by the parser. It consumes tokens based on their type and constructs the dictionary literal accordingly.
- **Error Handling**: Errors related to syntax issues are handled through the parser's error reporting mechanism, which may involve throwing exceptions or updating the error log.
- **AST Construction**: The function constructs an Abstract Syntax Tree (AST) node for each key-value pair in the dictionary. These nodes are then added to the `dict.pairs` vector.

## Detailed Explanation

### Parsing Process

1. **Initialization**:
   - The function starts by recording the current line number (`ln`).
   - It expects a left brace (`{`) at the current token position and skips any newlines before proceeding.

2. **Dictionary Literal Creation**:
   - An instance of `DictLiteral` named `dict` is created to store the parsed key-value pairs.

3. **Main Loop**:
   - The function enters a loop that continues until either a right brace (`}`) is encountered or the end of the input is reached.
   
4. **Handling Spread Operator**:
   - If the current token is an identifier followed by three dots (`...`), indicating a spread operator, the function consumes the spread operator and parses the subsequent expression using `parseUnary()`. This expression is then added to the dictionary with a `nullptr` key, serving as a sentinel to indicate a spread operation.
   - After consuming the spread operator, the function skips any newlines and checks if a comma follows. If not, it breaks out of the loop.

5. **Parsing Keys**:
   - The function attempts to parse keys for the dictionary. Keys can be:
     - Quoted strings (`"key"`).
     - Numbers (`42`).
     - Bare identifiers (`key`).
     - Type keywords (`Int`, `Float`).

6. **Constructing AST Nodes**:
   - For each valid key, the function creates an `ASTNode` containing a `StringLiteral` representation of the key.
   - If the key is followed immediately by a colon (`:`), it is treated as a normal key-value pair.
   - If the key is followed by a comma or a closing brace without a colon, it is considered a shorthand property, where the key name is used both as the key and the value.

7. **Loop Continuation**:
   - After processing each key-value pair, the function skips any newlines and checks if a comma follows. If so, it continues to the next pair; otherwise, it breaks out of the loop.

8. **Finalization**:
   - Once the loop completes, the function returns the constructed `DictLiteral`.

### Code Breakdown

```cpp
int ln = current().line; // Record the current line number
expect(TokenType::LBRACE, "Expected '{'"); // Expect a left brace
skipNewlines(); // Skip any newlines before proceeding
DictLiteral dict; // Create a dictionary literal to store pairs

while (!check(TokenType::RBRACE) && !atEnd()) // Continue until a right brace or end of input
{
    // Handle spread operator
    if (check(TokenType::IDENTIFIER) && current().value == "...") 
    {
        consume(); // Eat "..."
        auto spreadExpr = parseUnary(); // Parse the spread expression
        dict.pairs.emplace_back(nullptr, std::move(spreadExpr)); // Add to dictionary with null key
        skipNewlines(); // Skip any newlines
        if (!match(TokenType::COMMA)) // Check for comma
            break; // Break if no comma found
        skipNewlines(); // Skip any newlines
        if (check(TokenType::RBRACE)) // Check for closing brace
            break; // Break if closing brace found
        continue; // Continue to next iteration
    }

    // Parse keys
    ASTNodePtr key;
    bool isShorthand = false;
    if (check(TokenType::IDENTIFIER) || isCTypeKeyword(current().type) || check(TokenType::TYPE_STRING)) 
    {
        // Peek ahead to determine if it's a bare string key
        size_t la = pos + 1;
        while (la < tokens.size() && tokens[la].type == TokenType::NEWLINE)
            la++;

        if (la < tokens.size() && tokens[la].type == TokenType::COLON) 
        {
            // Normal key-value pair
            auto keyName = consume().value;
            key = std::make_unique<ASTNode>(StringLiteral{keyName}, ln);
        }
        else if (la < tokens.size() &&