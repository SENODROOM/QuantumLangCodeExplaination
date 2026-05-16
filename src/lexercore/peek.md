# `peek` Function

## Overview

The `peek` function is an essential utility method within the LexerCore class of the Quantum Language compiler. It allows developers to inspect characters at a specified position in the source code without altering the lexer's current state or advancing its cursor. This functionality is particularly useful during the lexical analysis phase when determining token types and boundaries.

## Parameters

- **pos**: The current position of the lexer in the source code.
- **offset**: An integer representing the number of positions ahead from the current position that you want to peek at. If `offset` is 0, it returns the character at the current position.

## Return Value

The function returns the character located at the position `pos + offset`. If the calculated position exceeds the bounds of the source code (`src.size()`), it returns the null character (`'\0'`). This ensures that the lexer can safely access characters beyond the end of the input stream without causing runtime errors.

## Edge Cases

1. **Offset Greater than Source Size**: When `offset` is greater than the size of the source code, the function correctly returns `'\0'`, indicating the end of the input stream.
2. **Negative Offset**: A negative offset would result in accessing characters before the start of the source code, which could lead to undefined behavior. However, since the function checks if the calculated position is less than the source size, it effectively handles negative offsets gracefully by returning `'\0'`.

## Interactions with Other Components

The `peek` function interacts closely with the LexerCore's internal state, specifically the `src` string which holds the entire source code. By allowing developers to look ahead at characters without modifying the lexer's position, it facilitates more complex parsing logic where multiple tokens need to be considered simultaneously.

Here’s how it might be used in the context of a lexer:

```cpp
char currentChar = lexer.peek(0); // Get the current character
if (currentChar == 'a') {
    char nextChar = lexer.peek(1); // Look ahead one character
    if (nextChar == 'b') {
        // Handle "ab" token
    } else {
        // Handle single "a" token
    }
} else {
    // Handle other tokens
}
```

In this example, the lexer uses `peek` to check both the current and the next character without advancing its position, enabling it to correctly identify and process multi-character tokens like `"ab"`.

Overall, the `peek` function plays a vital role in the Quantum Language compiler by providing a safe and efficient way to inspect characters in the source code, thereby supporting robust lexical analysis.