# `peek` Function

## Overview

The `peek` function is a crucial utility method within the LexerCore class of the Quantum Language compiler. Its primary purpose is to allow developers to examine characters at a specified position in the source code without changing the lexer's current state or advancing its position. This functionality is vital for lookahead operations during lexical analysis, enabling the lexer to make informed decisions based on upcoming tokens.

## Parameters

- **offset**: An integer representing the number of positions ahead in the source code that you want to inspect. The default value is 0, which means that the function will return the character at the current lexer position.

## Return Value

The `peek` function returns a single character (`char`) from the source code. If the specified position (`pos + offset`) is within the bounds of the source code, it returns the character at that position. Otherwise, it returns the null character (`'\0'`). This behavior ensures that the function can safely be used even when attempting to access characters beyond the end of the source code.

## Edge Cases

1. **Offset Greater than Source Code Length**: When the provided offset exceeds the length of the source code, the function returns the null character (`'\0'`). This prevents out-of-bounds errors and allows the lexer to handle unexpected input gracefully.
   
2. **Negative Offset**: A negative offset is treated as an attempt to look behind the current position. In such cases, the function also returns the null character (`'\0'`). This is because the lexer's design typically assumes a forward-only approach to tokenization, and looking behind would require significant changes to the lexer's internal state management.

## Interactions with Other Components

The `peek` function interacts closely with the LexerCore class, specifically with its member variables:
- **src**: A string representing the entire source code being analyzed.
- **pos**: An index indicating the current position within the source code where the lexer is focused.

By using the `peek` function, other parts of the compiler can perform lookahead operations without disrupting the lexer's progress. For example, a parser might use `peek` to check whether the next few characters form a valid keyword before deciding how to proceed with parsing.

Here is a brief overview of how the `peek` function is implemented:

```cpp
char peek(size_t offset = 0) const {
    size_t p = pos + offset; // Calculate the position to peek
    return p < src.size() ? src[p] : '\0'; // Return the character at the calculated position or null character if out of bounds
}
```

In summary, the `peek` function serves as a safe and efficient way to inspect characters in the source code without affecting the lexer's state. Its implementation ensures robustness against various edge cases, making it a valuable tool for the Quantum Language compiler's lexical analysis process.