# `readString` Function

## Overview

The `readString` function is responsible for lexically analyzing and extracting string literals from the source code being processed by the Quantum Language compiler's lexer. This function ensures that string literals are correctly identified and handled, including support for escape sequences within strings.

### Why It Works This Way

The function operates by advancing through the source code character by character until it encounters the closing quote of the string. During this process, it checks for escape sequences prefixed by a backslash (`\`). Each recognized escape sequence is replaced by its corresponding character in the resulting string token. If an unrecognized escape sequence is encountered, it is added to the string as-is. This approach allows the function to accurately parse both simple and complex string literals.

### Parameters/Return Value

- **Parameters**:
  - None
  
- **Return Value**:
  - Returns a `Token` object representing the extracted string literal. The token includes the type (`TokenType::STRING`), the value of the string, and the starting line and column positions where the string was found in the source code.

### Edge Cases

1. **Empty String**: If the string literal is empty (i.e., two consecutive quotes without any content between them), the function will still correctly identify and handle it.
2. **Unrecognized Escape Sequence**: If an escape sequence is not recognized (e.g., `\x`, `\z`), it is treated as a regular character within the string.
3. **Unterminated String Literal**: If the string literal is not properly terminated (i.e., it ends before encountering the closing quote), the function throws a `QuantumError` with the message "LexError: Unterminated string literal" along with the starting line position.

### Interactions With Other Components

- **Lexer Class**: The `readString` function is part of the Lexer class, which is responsible for breaking down the source code into individual tokens. It interacts with the Lexer's state variables such as `line`, `col`, and `pos` to keep track of the current position in the source code.
- **Tokenizer**: When a string literal is successfully read, the `readString` function creates a `Token` object and passes it up to the higher-level tokenizer for further processing.
- **Error Handling**: If an error occurs during the reading of a string literal (such as an unterminated string), the function throws a `QuantumError`. This error is then caught and handled by the appropriate error management component of the compiler.

Overall, the `readString` function plays a crucial role in the lexical analysis phase of the Quantum Language compiler, ensuring that string literals are correctly parsed and represented in the token stream.