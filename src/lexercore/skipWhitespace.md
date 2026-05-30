# `skipWhitespace`

## Purpose

The `skipWhitespace` function is designed to consume and skip any whitespace characters in the source code being processed by the Quantum Language compiler's lexer. This includes spaces (' '), tabs ('\t'), and carriage returns ('\r').

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: None

The function operates directly on the internal state of the lexer, modifying its position (`pos`) to skip over whitespace characters without returning any value.

## How It Works

The function iterates through the source code string (`src`) starting from the current position (`pos`). During each iteration, it checks if the character at the current position is a whitespace character (space, tab, or carriage return). If it is, the function advances the position (`pos`) by one using the `advance()` method. This process continues until a non-whitespace character is encountered or the end of the source code is reached.

### Why It Works This Way

This approach ensures that all leading and trailing whitespace characters are skipped efficiently. By iterating through the source code and checking each character individually, the function can handle various edge cases such as multiple consecutive whitespace characters or whitespace characters interspersed with valid tokens.

## Edge Cases

1. **Multiple Consecutive Whitespace Characters**: The function will skip over all consecutive whitespace characters in a single pass, ensuring that only one token is generated even if there are multiple spaces between words.
2. **End of Source Code**: If the end of the source code is reached before encountering a non-whitespace character, the function will simply exit without performing any action.
3. **Empty Source Code**: In the case where the source code is empty, the function will also terminate immediately without attempting to access invalid memory locations.

## Interactions With Other Components

The `skipWhitespace` function interacts closely with the lexer's internal state, particularly the `pos` variable which tracks the current position in the source code. After skipping whitespace, the lexer may proceed to identify and tokenize the next valid sequence of characters. Additionally, the function may be called multiple times during the lexing process to ensure that all whitespace is removed before tokenization begins.