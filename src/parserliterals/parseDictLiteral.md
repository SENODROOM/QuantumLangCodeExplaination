# `parseDictLiteral` Function

## Overview

The `parseDictLiteral` function in the Quantum Language compiler is designed to parse dictionary literals from the source code. Dictionary literals are enclosed within curly braces `{}` and consist of key-value pairs. This function ensures that the parsed dictionary literal is correctly structured and handles various syntax cases such as shorthand properties and spread operators.

### Parameters

- **None**: The function operates directly on the global state of the parser, which includes the current position (`pos`) in the token stream and the list of tokens (`tokens`). It does not take any explicit parameters.

### Return Value

- **`DictLiteral`**: The function returns an instance of `DictLiteral`, which represents the parsed dictionary literal. This object contains a vector of pairs, where each pair consists of a key and its corresponding value.

### Edge Cases

- **Empty Dictionary**: If the parser encounters an empty dictionary literal `{}`, it will return an empty `DictLiteral`.
- **Spread Operator**: The function supports the spread operator `{...obj, key: val}`, allowing dictionaries to be dynamically constructed from existing objects.
- **Syntax Errors**: If the parser encounters unexpected tokens or malformed syntax, it will throw an appropriate error message indicating the issue.

### Interactions with Other Components

- **Token Stream**: The function relies on the global token stream (`tokens`) to extract and process tokens. It uses functions like `current()` to get the current token and `consume()` to advance the token pointer.
- **Error Handling**: The function interacts with the error handling mechanism of the compiler to report syntax errors when necessary. Functions like `expect()` and `match()` are used to validate the expected tokens.
- **AST Construction**: The function constructs an Abstract Syntax Tree (AST) representation of the parsed dictionary literal. It creates instances of `ASTNode` and assigns them to the `dict.pairs` vector.

### Detailed Explanation

The `parseDictLiteral` function begins by recording the current line number (`ln`) for error reporting purposes. It then expects the opening brace `{` token, throwing an error if it is not found.

```cpp
int ln = current().line;
expect(TokenType::LBRACE, "Expected '{'");
```

After consuming the opening brace, the function skips any newlines to ensure proper parsing of the dictionary content.

```cpp
skipNewlines();
```

A `DictLiteral` object is initialized to store the parsed key-value pairs.

```cpp
DictLiteral dict;
```

The function enters a loop that continues until either the closing brace `}` is encountered or the end of the token stream is reached.

```cpp
while (!check(TokenType::RBRACE) && !atEnd())
```

Inside the loop, the function checks for the spread operator `{...obj, key: val}`. If found, it consumes the spread operator token, parses the expression following it, and adds a special sentinel pair to the dictionary.

```cpp
if (check(TokenType::IDENTIFIER) && current().value == "...")
{
    consume(); // eat "..."
    auto spreadExpr = parseUnary();
    dict.pairs.emplace_back(nullptr, std::move(spreadExpr));
    skipNewlines();
    if (!match(TokenType::COMMA))
        break;
    skipNewlines();
    if (check(TokenType::RBRACE))
        break;
    continue;
}
```

If the spread operator is not present, the function proceeds to parse the key. The key can be a quoted string, a number, a bare identifier, or a type keyword.

```cpp
// Key: accept quoted string, number, bare identifier, or type keyword
// e.g.  "name": ...   or   firstName: ...   or   42: ...
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
        key = parseExp();
```

For non-shorthand keys, the function calls `parseExp()` to parse the value associated with the key.

```cpp
if (!isShorthand)
    dict.pairs.emplace_back(std::move(key), parseExp());
else
    dict.pairs.emplace_back(std::move(key), nullptr);
```

After processing a key-value pair, the function skips any newlines and checks for a comma `,`. If a comma is found, it continues to the next key-value pair; otherwise, it breaks out of the loop.

```cpp
skipNewlines();
if (!match(TokenType::COMMA))
    break;
skipNewlines();
```

Finally, the function consumes the closing brace `}` token and returns the parsed `DictLiteral`.

```cpp
consume(); // eat '}'
return dict;
```

This comprehensive approach ensures that the `parseDictLiteral` function accurately handles various dictionary