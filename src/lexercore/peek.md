# `peek` Function

## Overview

The `peek` function is an essential utility method within the LexerCore class of the Quantum Language compiler. It allows developers to inspect characters at a specified position in the source code without altering the lexer's current state or advancing its cursor. This functionality is crucial for lookahead operations during lexical analysis, enabling the lexer to determine the next token based on upcoming characters.

## Parameters and Return Value

- **Parameters**:
  - `pos`: The current position in the source code where the lexer is located.
  - `offset`: An integer representing the number of positions ahead from the current position to inspect.

- **Return Value**:
  - Returns the character at the specified position (`pos + offset`) in the source code as a `char`.
  - If the calculated position exceeds the bounds of the source code, it returns the null character (`'\0'`).

## Edge Cases

1. **Offset Greater than Source Length**: When the `offset` parameter is greater than the length of the source code, the function correctly returns `'\0'`, indicating that there are no more characters to peek at beyond the end of the input.

2. **Negative Offset**: A negative `offset` will result in a position calculation that points before the start of the source code. In such cases, the function also returns `'\0'`. This behavior ensures that the lexer can safely handle unexpected negative offsets without causing errors.

3. **Empty Source Code**: If the source code is empty (`src.size() == 0`), the function will always return `'\0'`, reflecting the absence of any characters to inspect.

## Interactions with Other Components

The `peek` function interacts closely with the LexerCore class, which manages the overall state of the lexer including the current position (`pos`). By using the `peek` function, the lexer can perform various tasks such as:

- **Token Recognition**: During the recognition of tokens, the lexer might need to look ahead several characters to identify the correct token type. For example, recognizing a keyword like "if" requires checking subsequent characters to ensure they do not form part of another identifier or string.
  
- **Error Handling**: The lexer uses `peek` to check for potential syntax errors by examining characters ahead. If an invalid sequence of characters is encountered, the lexer can report an error without consuming any characters.

- **Comment Parsing**: Comments in the source code often span multiple lines. The lexer uses `peek` to detect comment delimiters and skip over entire comments without parsing them as tokens.

In summary, the `peek` function is a vital component of the LexerCore class, facilitating lookahead operations necessary for accurate lexical analysis. Its design ensures robust handling of various edge cases, making it a reliable tool for maintaining the lexer's state and advancing through the source code efficiently.