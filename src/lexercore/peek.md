# `peek` Function

## Overview

The `peek` function is an essential utility method within the LexerCore class of the Quantum Language compiler. It allows developers to inspect characters at a specified position in the source code without altering the lexer's current state or advancing its cursor. This functionality is crucial for lookahead operations during lexical analysis, enabling the parser to make informed decisions based on upcoming tokens.

## Parameters and Return Value

- **Parameters**:
  - `size_t offset`: The number of positions ahead in the source code from the current lexer position (`pos`) that you want to inspect. A positive value means looking ahead, while a negative value means looking backward.

- **Return Value**:
  - `char`: The character at the specified position relative to the current lexer position. If the specified position is out of bounds (either before the start of the source code or after the end), the function returns the null character (`'\0'`).

## How It Works

The `peek` function calculates the absolute position in the source code by adding the `offset` to the current lexer position (`pos`). It then checks if this calculated position is within the valid range of the source code (i.e., between 0 and `src.size()`). If it is within the valid range, the function returns the character at that position. Otherwise, it returns the null character (`'\0'`).

This design ensures that the lexer remains in its current state and only provides information about future or past characters as requested. By returning the null character when the specified position is out of bounds, it prevents potential runtime errors and makes the function safe to use even for edge cases where the offset might be too large or too small.

## Edge Cases

1. **Offset Greater Than Source Size**: When the `offset` is greater than the size of the source code, the function will return the null character (`'\0'`). This handles situations where the lexer attempts to look ahead beyond the end of the input.

2. **Offset Less Than Zero**: When the `offset` is less than zero, the function will also return the null character (`'\0'`). This handles scenarios where the lexer tries to look backward before the beginning of the input.

3. **Empty Source Code**: If the source code is empty (`src.size() == 0`), any non-zero offset will result in a return value of the null character (`'\0'`).

4. **Current Position at End of Source Code**: If the current lexer position (`pos`) is already at the end of the source code, attempting to peek forward will also return the null character (`'\0'`).

## Interactions with Other Components

The `peek` function interacts closely with the LexerCore class, which manages the overall state of the lexer including the current position (`pos`) and the source code (`src`). By providing a way to inspect characters without advancing the lexer, it facilitates more complex parsing algorithms that require lookahead capabilities.

For example, the parser might need to determine whether the next token should be a keyword or an identifier. In such cases, it can use the `peek` function to examine the next few characters and decide accordingly without changing the lexer's state.

In summary, the `peek` function is a vital tool for lookahead operations in the Quantum Language compiler. Its implementation ensures that the lexer remains unchanged while providing necessary information to support more sophisticated parsing logic.