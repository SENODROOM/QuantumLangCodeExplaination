# `current` Function

## Overview

The `current` function is an essential method of the LexerCore class in the Quantum Language compiler. It fetches the character at the position indicated by the `pos` cursor within the source code string `src`. If the cursor has reached or surpassed the end of the string, the function returns the null character (`'\0'`). This ensures that the lexer can safely access characters without causing out-of-bounds errors.

## Parameters

- **None**

## Return Value

- **char**: The character at the current position of the `pos` cursor. Returns `'\0'` if the cursor is past the end of the source code string.

## Edge Cases

1. **Empty String**: If the source code string `src` is empty, calling `current()` will return `'\0'`.
2. **Cursor Beyond End**: When the `pos` cursor exceeds the length of the source code string, `current()` returns `'\0'`, preventing any potential runtime errors due to accessing invalid memory locations.

## Interactions with Other Components

The `current` function interacts closely with the LexerCore's state management and tokenization process. It is used internally to check the next character in the source code during lexical analysis. For example, when determining whether a sequence of characters forms a keyword or an identifier, the lexer uses `current()` to look ahead at subsequent characters.

Here’s how it might be used in a typical scenario:

```cpp
// Example usage within LexerCore::nextToken()
if (isalpha(current())) {
    // Start of an identifier or keyword
    std::string token;
    while (isalnum(current()) || current() == '_') {
        token += current();
        advance();  // Move to the next character
    }
    // Check if the token is a keyword
    if (isKeyword(token)) {
        return Token(TokenType::KEYWORD, token);
    } else {
        return Token(TokenType::IDENTIFIER, token);
    }
} else {
    // Handle other types of tokens or errors
    return Token(TokenType::ERROR, "Unexpected character");
}
```

In this example, `current()` is used to determine if the first character of a potential token is alphabetic, which indicates the start of either an identifier or a keyword. The function continues to collect characters until it encounters a non-alphanumeric character, then checks if the collected token matches any known keywords. This demonstrates the function's role in driving the lexer's decision-making process based on the current character in the input stream.