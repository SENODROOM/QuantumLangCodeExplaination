# `peek` Function

## Overview

The `peek` function is a utility method within the LexerCore class of the Quantum Language compiler. Its primary purpose is to examine the character at a specified position in the source code without advancing the current position in the lexer.

## Parameters and Return Value

- **Parameters**:
  - `offset`: A `size_t` type parameter that specifies how many characters ahead from the current lexer position (`pos`) the function should look.

- **Return Value**:
  - The function returns a `char` type representing the character at the calculated position (`pos + offset`). If the calculated position exceeds the bounds of the source code string (`src.size()`), the function returns the null character (`'\0'`).

## How It Works

The `peek` function calculates the position it would be at if it advanced by the given `offset`. It then checks if this calculated position is within the valid range of indices for the source code string (`src`). If the position is valid, it returns the character at that position; otherwise, it returns the null character.

This design allows the lexer to preview future characters in the source code without altering its state, which is crucial for parsing operations where lookahead is necessary.

## Edge Cases

1. **Offset Greater Than or Equal to Source Code Length**: When `offset` is greater than or equal to the length of the source code string, the function will return the null character (`'\0'`). This prevents out-of-bounds access and ensures that the lexer can handle unexpected end-of-file scenarios gracefully.

2. **Negative Offset**: Although not explicitly handled in the provided code snippet, negative offsets could potentially lead to undefined behavior because they might result in accessing memory before the beginning of the string. In practice, the lexer implementation likely includes checks to prevent such usage.

## Interactions With Other Components

- **Lexer State Management**: The `peek` function operates on the current state of the lexer, specifically the `pos` member variable which tracks the current position in the source code. It does not modify this state, allowing the lexer to continue parsing from the same point after a call to `peek`.

- **Parsing Operations**: The `peek` function is often used in parsing algorithms to determine the next token or to check for certain patterns in the source code. For example, when parsing an integer literal, the lexer might use `peek` to check if the next character is a digit before deciding whether to continue reading the number.

Here's a brief example of how `peek` might be used in a parsing context:

```cpp
// Assuming 'lexer' is an instance of LexerCore
if (isdigit(lexer.peek())) {
    // Read and parse an integer literal
    int value = lexer.parseInt();
} else {
    // Handle the case where the next token is not an integer literal
    lexer.advance(); // Move to the next character
}
```

In summary, the `peek` function is a fundamental utility in the LexerCore class, enabling safe lookahead into the source code without changing the lexer's state. Its correct handling is essential for the proper functioning of parsing operations in the compiler.