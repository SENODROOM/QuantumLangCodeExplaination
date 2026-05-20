# `current` Function

## Overview

The `current` function is an essential method of the LexerCore class in the Quantum Language compiler. It retrieves the character located at the current position (`pos`) within the source code string (`src`). This function plays a critical role in parsing and tokenizing the input source code by allowing the lexer to access individual characters as needed.

### Why It Works This Way

The implementation of the `current` function checks whether the current position (`pos`) is less than the size of the source code string (`src`). If it is, the function returns the character at that position. Otherwise, it returns the null character (`'\0'`). This design ensures that the lexer can safely access characters without causing out-of-bounds errors. The use of the null character helps in distinguishing between valid characters and the end of the string.

## Parameters

- **None**: The `current` function does not take any parameters.

## Return Value

- **char**: Returns the character at the current position (`pos`) within the source code string (`src`). If the cursor has reached or surpassed the end of the string, it returns the null character (`'\0'`).

## Edge Cases

1. **Empty Source Code String**: If the source code string (`src`) is empty, calling `current()` will return the null character (`'\0'`).
2. **Position Beyond String Length**: If the current position (`pos`) exceeds the length of the source code string (`src`), the function will also return the null character (`'\0'`).

## Interactions With Other Components

The `current` function interacts closely with the `LexerCore` class's state, particularly the `pos` cursor which indicates the current position in the source code string being processed. This function is used extensively throughout the lexer's operations to read and analyze individual characters, aiding in the creation of tokens and the overall parsing process.

Here is a brief snippet demonstrating how `current` might be used within the `LexerCore` class:

```cpp
// Example usage within LexerCore class
void LexerCore::processCharacter() {
    char ch = current(); // Get the current character
    if (ch == '\0') {
        // End of string reached
        return;
    }
    // Process the character
    pos++; // Move to the next position
}
```

In summary, the `current` function is a fundamental utility in the Quantum Language compiler's lexer core, enabling safe and efficient access to characters in the source code. Its straightforward implementation makes it easy to integrate into various parsing and tokenization processes, contributing significantly to the lexer's functionality.