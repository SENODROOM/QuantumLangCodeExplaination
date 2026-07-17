# `skipWhitespace`

## Purpose

The `skipWhitespace` function is essential for the Quantum Language compiler's lexer to correctly parse the input source code. It consumes and skips over any whitespace characters encountered during lexical analysis, including spaces (' '), tabs ('\t'), and carriage returns ('\r'). This ensures that the lexer can focus on meaningful tokens rather than being distracted by formatting issues.

## Functionality

The function operates by iterating through the source code starting from the current position (`pos`). For each character at the current position, it checks if it is one of the whitespace characters specified: space, tab, or carriage return. If the character is a whitespace, the function advances the position pointer (`advance()`), effectively skipping over the whitespace character. This process continues until a non-whitespace character is found or the end of the source code is reached.

## Why It Works This Way

This implementation ensures that all whitespace characters are skipped efficiently without prematurely terminating the tokenization process. By using a loop that continues as long as the current character is a whitespace, the function handles multiple consecutive whitespace characters seamlessly. The use of `advance()` allows the lexer to move forward to the next character after skipping the current whitespace, ensuring continuous progress through the source code.

## Parameters/Return Value

- **Parameters**:
  - None. The function operates directly on the internal state of the lexer, specifically the `src` string and the `pos` position pointer.

- **Return Value**:
  - `void`. The function does not return any value; instead, it modifies the internal state of the lexer by advancing the position pointer.

## Edge Cases

- **Empty Source Code**: If the source code is empty (`src.size() == 0`), the function will immediately exit without performing any operations, as there are no characters to skip.
- **End of Source Code**: When the end of the source code is reached (`pos >= src.size()`), the function stops iterating and exits, leaving the position pointer at the end of the source code.
- **Mixed Whitespace Characters**: The function correctly identifies and skips all types of whitespace characters (space, tab, carriage return) in any combination within the source code.

## Interactions with Other Components

The `skipWhitespace` function interacts closely with the lexer's main state variables (`src`, `pos`) and methods (`current()`, `advance()`). It is typically called before attempting to read a new token, ensuring that any leading whitespace is ignored. This interaction is crucial for maintaining accurate parsing of the source code, as it prevents the lexer from interpreting whitespace as part of a token.

In summary, the `skipWhitespace` function is a vital component of the Quantum Language compiler's lexer, responsible for efficiently consuming and skipping whitespace characters. Its design ensures robust handling of various edge cases and seamless integration with other lexer functionalities, contributing to the overall reliability and accuracy of the compiler's tokenization process.