# `skipWhitespace`

## Purpose

The `skipWhitespace` function is designed to consume and skip any whitespace characters in the source code being processed by the Quantum Language compiler's lexer. This includes spaces (' '), tabs ('\t'), and carriage returns ('\r'). The primary purpose of this function is to ensure that the lexer can correctly identify tokens without being misled by unnecessary whitespace.

## Functionality

The function operates by iterating through the source code starting from the current position (`pos`). It checks each character at the current position using the `current()` method. If the character is a space, tab, or carriage return, the `advance()` method is called to move the position forward one character. This process continues until a non-whitespace character is encountered or the end of the source code is reached (`pos >= src.size()`).

### Parameters
- None

### Return Value
- None

### Edge Cases
1. **Empty Source Code**: If the source code is empty (`src.size() == 0`), the function will immediately exit as there are no characters to process.
2. **End of Source Code**: If the end of the source code is reached before encountering a non-whitespace character, the function will stop processing.
3. **Non-Whitespace Characters**: If the current character is not a space, tab, or carriage return, the function will terminate early, leaving the lexer ready to process the next token.

### Interactions with Other Components
The `skipWhitespace` function interacts closely with the lexer's state machine. It is typically invoked at the beginning of the tokenization process for each new character read from the source code. By skipping over whitespace characters, it ensures that the subsequent tokenization logic focuses only on meaningful characters, thereby simplifying the parsing process.

Additionally, the `skipWhitespace` function may be used in conjunction with other utility functions within the lexer, such as `matchToken`, to handle more complex scenarios where whitespace might affect the interpretation of tokens. For example, comments in some languages start with a sequence of whitespace followed by special characters like `//` or `/*`.

Here is a simplified version of how `skipWhitespace` might be integrated into the lexer's main loop:

```cpp
while (pos < src.size()) {
    skipWhitespace(); // Skip any leading whitespace
    char ch = current(); // Get the next character
    if (ch == '\0') break; // End of input
    Token* token = matchToken(ch); // Attempt to match a token
    if (!token) {
        error("Unexpected character");
        break;
    }
    // Process the matched token
    pos += token->length();
}
```

In this example, after advancing past any initial whitespace, the lexer attempts to match the next token. If a token cannot be matched, an error is reported, and the loop terminates. This interaction highlights the importance of `skipWhitespace` in maintaining the lexer's ability to accurately parse the source code.