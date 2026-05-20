# `peek` Function

## Overview

The `peek` function is an essential utility method within the LexerCore class of the Quantum Language compiler. It allows developers to inspect characters at a specified position in the source code without altering the lexer's current state or advancing its cursor. This functionality is crucial for lookahead operations during lexical analysis, enabling the parser to make informed decisions based on upcoming tokens.

## Parameters and Return Value

- **Parameters**:
  - `offset`: A `size_t` parameter that specifies how many positions ahead from the current lexer position (`pos`) you want to inspect the character. If `offset` is 0, it returns the character at the current lexer position.

- **Return Value**:
  - The function returns a `char`. If the specified position (`pos + offset`) is within the bounds of the source code (`src`), it returns the character at that position. Otherwise, it returns the null character (`'\0'`).

## How It Works

The `peek` function calculates the position to inspect by adding the `offset` to the current lexer position (`pos`). It then checks if this calculated position is within the bounds of the source code string (`src`). If it is, the function retrieves and returns the character at that position. If the calculated position exceeds the size of the source code string, indicating that there are no more characters to inspect, the function returns the null character (`'\0'`). This behavior ensures that the lexer remains in its current state and can safely perform lookahead operations without affecting subsequent parsing steps.

## Edge Cases

1. **Offset Greater Than Source Size**: When the `offset` is greater than the size of the source code string, the function correctly returns the null character (`'\0'`). This prevents out-of-bounds access and potential runtime errors.

2. **Offset Equals Zero**: When `offset` is zero, the function returns the character at the current lexer position. This is useful for scenarios where you need to check the character at the current position without moving forward.

3. **Empty Source Code**: If the source code string (`src`) is empty, the function will always return the null character (`'\0'`). This handles the case where there are no characters to inspect gracefully.

## Interactions with Other Components

The `peek` function interacts closely with the LexerCore class, which manages the overall lexical analysis process. By providing a safe way to inspect characters without advancing the lexer, it facilitates the implementation of complex parsers and grammars that require lookahead capabilities. For example, when parsing expressions, the lexer might use `peek` to determine whether to continue parsing as part of the same expression or to start a new one based on the next token.

In summary, the `peek` function is a vital tool for performing lookahead operations in the Quantum Language compiler's lexical analyzer. Its design ensures that the lexer remains in its current state while allowing developers to inspect characters at any desired position, enhancing the flexibility and robustness of the compiler's parsing capabilities.