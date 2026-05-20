# `readString` Function

## Overview

The `readString` function is designed to lexically analyze and extract string literals from the source code being processed by the Quantum Language compiler's lexer. It ensures that string literals are correctly identified and handled, including support for escape sequences within the strings.

### Why It Works This Way

This implementation of `readString` works by iterating through the source code starting from the position immediately after the opening quote of the string. It reads characters until it encounters the closing quote, handling escape sequences as specified. If an escape sequence is encountered, it processes the character following the backslash according to the rules defined in the switch statement. For example, `\n` is replaced with a newline character, `\t` with a tab, and so on. If the end of the source code is reached without encountering a closing quote, it throws an error indicating an unterminated string literal.

### Parameters/Return Value

- **Parameters**:
  - None

- **Return Value**:
  - Returns a `Token` object representing the extracted string literal. The token type is set to `TokenType::STRING`, and it includes the string content, the starting line number, and the starting column number.

### Edge Cases

1. **Unterminated String Literal**: If the source code ends before a closing quote is found, the function throws an exception with the message "LexError: Unterminated string literal". This handles cases where a string literal is not properly closed, which could lead to syntax errors later in the compilation process.

2. **Escape Sequences**: The function supports various escape sequences such as `\n`, `\t`, `\r`, `\\`, `'`, `"`, and `\0`. Handling these correctly ensures that the string literal is interpreted accurately by the compiler.

3. **Empty String Literals**: While not explicitly handled in the provided code snippet, the function can be extended to recognize and handle empty string literals (`""`). This would involve checking if `str` is empty at the end and returning a corresponding token.

### Interactions With Other Components

- **Lexer Class**: The `readString` function is part of the `Lexer` class, which is responsible for breaking down the source code into tokens. It interacts with the `advance` method to move through the source code and retrieve the current character.

- **Token Class**: The function returns a `Token` object, which is used throughout the compiler to represent different types of lexical elements. The `Token` class likely contains methods for accessing the token type, value, and location information.

- **Error Handling**: If an error occurs during the reading of a string literal, such as an unterminated string, the function throws a `QuantumError` with appropriate details. This error is then caught and handled by higher-level components of the compiler.

By carefully managing the extraction and interpretation of string literals, the `readString` function plays a crucial role in ensuring that the Quantum Language compiler can correctly parse and process the source code.