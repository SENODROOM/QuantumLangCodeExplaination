# `current` Function

## Overview

The `current` function is a crucial method within the LexerCore class of the Quantum Language compiler. Its primary purpose is to fetch the character currently being pointed to by the lexer's internal position pointer (`pos`) within the source code string (`src`). The function returns this character, facilitating further lexical analysis and tokenization processes.

## Parameters

- **None**

## Return Value

- **char**: The character at the current position in the source code string. If the position exceeds the bounds of the string, the function returns the null character (`'\0'`).

## Edge Cases

1. **Position Out of Bounds**: When the internal position pointer (`pos`) points beyond the end of the source code string (`src`), the function returns `'\0'`. This ensures that the lexer can safely handle out-of-bounds access without causing runtime errors or undefined behavior.

2. **Empty Source Code String**: If the source code string (`src`) is empty, the function will return `'\0'` immediately, as there are no characters to retrieve.

## Interactions with Other Components

The `current` function interacts closely with the LexerCore's state management, particularly the `pos` variable which tracks the lexer's current position in the source code. This function is typically used in conjunction with other methods such as `next`, `peek`, and `advance` to navigate through the source code and extract meaningful tokens.

Here’s how `current` might be used in a typical lexer workflow:

```cpp
// Example usage of current in a lexer loop
while (!isEndOfInput()) {
    char ch = current();
    if (ch == ' ') {
        skipWhitespace(); // Skip spaces
    } else if (isDigit(ch)) {
        parseNumber(); // Parse numbers
    } else if (isAlpha(ch)) {
        parseIdentifier(); // Parse identifiers
    } else {
        throw LexicalError("Unexpected character encountered"); // Handle unexpected characters
    }
    advance(); // Move to the next character
}

bool isEndOfInput() {
    return pos >= src.size();
}

void skipWhitespace() {
    while (current() == ' ') {
        advance();
    }
}

void parseNumber() {
    // Logic to parse numbers
    advance();
}

void parseIdentifier() {
    // Logic to parse identifiers
    advance();
}

void advance() {
    ++pos;
}
```

In summary, the `current` function is vital for the Quantum Language compiler's lexer component, providing a safe and efficient means to access characters in the source code string during the parsing process. By handling edge cases appropriately and interacting seamlessly with other lexer functions, it ensures robust and accurate tokenization.