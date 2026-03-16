# readString Function - String Literal Tokenization

## Overview
The `readString()` function processes string literals enclosed in either single or double quotes within the source code. It handles escape sequences and detects unterminated strings to ensure proper tokenization during compilation.

### Parameters
- None

### Return Value
- Returns a `Token` object representing the string literal. The token type is `TokenType::STRING`, and it contains the string value, starting line, and starting column.

### Edge Cases
1. **Unterminated Strings**: If the string literal is not terminated before reaching the end of the source code, the function throws a `QuantumError` with the message "LexError: Unterminated string literal" at the position where the string started.
2. **Escape Sequences**: The function supports common escape sequences like `\n` (newline), `\t` (tab), `\r` (carriage return), `\\` (backslash), `\'` (single quote), `\"` (double quote), and `\0` (null character).

### Interactions with Other Components
- **Lexer Class**: The `readString()` function is part of the Lexer class, which is responsible for breaking down the source code into tokens.
- **Tokenizer Logic**: It interacts with the tokenizer logic to identify and process string literals, ensuring they are correctly recognized and handled in the parsing phase.
- **Error Handling**: When an unterminated string is detected, it communicates with the error handling component to report the issue, preventing further processing until the error is resolved.

### Detailed Explanation
The function begins by recording the starting line and column positions using `startLine` and `startCol`. It then advances past the opening quote character using `advance()`.

A loop continues to process characters until the closing quote is encountered or the end of the source code is reached. Inside the loop:

- **Character Check**: The function checks if the current character is a backslash (`\`). If so, it indicates an escape sequence.
- **Escape Sequence Handling**: Depending on the character following the backslash, the function appends the corresponding special character to the string (`str`). For example, `\n` is replaced with a newline character.
- **Regular Character Addition**: If the current character is not a backslash, it is directly appended to the string.

After exiting the loop, the function checks if the end of the source code was reached without finding the closing quote. If so, it throws a `QuantumError` indicating an unterminated string literal.

Finally, the function advances past the closing quote and returns a new `Token` object of type `STRING` containing the processed string along with its starting line and column positions.