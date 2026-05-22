# readString Function

## Overview

The `readString` function is designed to lexically analyze and extract string literals from the source code being processed by the Quantum Language compiler's lexer. It ensures that string literals are correctly identified and handled, including support for escape sequences within the strings.

### Why It Works This Way

This implementation of `readString` works by iterating through the source code starting from the first character after the opening quote. It handles escape sequences by checking for backslashes (`\`) and then interpreting the next character according to the standard escape sequence rules. For example, `\n` is replaced with a newline character, `\t` with a tab character, etc. If an unrecognized escape sequence is encountered, it is added to the string as-is. The function continues until it reaches the closing quote or encounters the end of the source code, at which point it throws an error indicating an unterminated string literal.

### Parameters/Return Value

- **Parameters**:
  - None

- **Return Value**:
  - A `Token` object representing the extracted string literal. The token includes the type (`TokenType::STRING`), the value of the string, and the starting line and column in the source code.

### Edge Cases

1. **Unterminated String Literal**: If the function reaches the end of the source code without encountering a closing quote, it throws a `QuantumError` with the message "Unterminated string literal" along with the starting line and column information.
2. **Escape Sequences**: The function correctly interprets common escape sequences like `\n`, `\t`, `\r`, `\\`, `'`, `"`, and `\0`. Any unrecognized escape sequence is treated as part of the string.
3. **Empty String**: An empty string literal between two quotes (`""`) will be correctly recognized and returned as a `Token`.

### Interactions With Other Components

The `readString` function interacts primarily with the lexer component of the Quantum Language compiler. It uses the `advance()` method to move through the source code, checking each character to determine whether it is part of the string or an escape sequence. Once the entire string has been read, it returns a `Token` object containing the string value, allowing the lexer to continue processing the remaining tokens in the source code.