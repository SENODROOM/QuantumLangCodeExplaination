# peek() Function - Lookahead Character Access

## Overview
The `peek()` function in the Quantum Language compiler is designed to provide a mechanism for non-destructive lookahead into the source code string. This means that the lexer can examine characters ahead of its current position without changing the lexer's internal state. This functionality is crucial for parsing and tokenizing the source code effectively.

## Parameters/Return Value
- **Parameters**: 
  - `offset` (size_t): The number of characters to look ahead from the current position.
  
- **Return Value**:
  - Returns the character at the specified lookahead position if it exists; otherwise, returns `\0` (null character).

## How It Works
The `peek()` function calculates the position (`p`) where the lookahead should occur by adding the `offset` to the current position (`pos`). If the calculated position is within the bounds of the source code string (`src`), it returns the character at that position. Otherwise, it returns `\0`, indicating that there are no more characters to look ahead.

This implementation ensures that the lexer remains in a consistent state after calling `peek()`, allowing subsequent calls to `get()` or other functions to continue from the same point as before.

## Edge Cases
1. **Offset Greater Than String Length**: When the `offset` exceeds the length of the source code string, `peek()` will return `\0`. This handles scenarios where the lexer attempts to access characters beyond the end of the input.
2. **Current Position At End Of String**: If the current position (`pos`) is already at the end of the source code string, `peek()` will also return `\0`. This prevents out-of-bounds errors when trying to access characters past the last one.

## Interactions With Other Components
- **Lexer State Management**: By not modifying the lexer's state, `peek()` allows other parts of the compiler to rely on the lexer's position being unchanged after a call to `peek()`.
- **Parsing Algorithms**: Functions like `match()` and `tokenize()` use `peek()` to determine the next sequence of characters and decide how to proceed with parsing.
- **Error Handling**: In error reporting, `peek()` helps identify the context around an error, providing more accurate diagnostics.

In summary, the `peek()` function is a vital tool in the Quantum Language compiler for managing lookahead operations efficiently and ensuring the lexer's state remains intact during parsing processes.