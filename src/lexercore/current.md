# `current` Function

## Overview

The `current` function is a crucial method within the LexerCore class of the Quantum Language compiler. Its primary role is to fetch the character located at the current position (`pos`) within the source code string (`src`). This function plays a pivotal part in the parsing and tokenization processes by providing access to individual characters as the lexer progresses through the input text.

### Parameters

- **None**: The `current` function does not take any parameters.

### Return Value

- **Character (`char`)**: If the current position (`pos`) is less than the size of the source code string (`src`), the function returns the character at that position. Otherwise, it returns the null character (`'\0'`).

### Edge Cases

1. **Position Out of Bounds**:
   - When the current position (`pos`) exceeds or equals the size of the source code string (`src`), the function returns the null character (`'\0'`). This prevents accessing memory outside the bounds of the string, which could lead to undefined behavior or crashes.

2. **Empty Source Code String**:
   - If the source code string (`src`) is empty, the function will immediately return the null character (`'\0'`). This ensures that there is always a valid character returned, even when the input is empty.

### Interactions with Other Components

- **LexerCore Class**: The `current` function is a fundamental component of the LexerCore class. It is used extensively throughout the parsing process to read individual characters one by one. The LexerCore class uses this function to advance its position, check for specific characters, and build tokens based on the sequence of characters encountered.

- **Parsing Process**: During the parsing phase, the LexerCore class relies on the `current` function to determine the next character in the source code. Based on this character, the lexer decides whether to continue reading, skip whitespace, or create a new token. For example, if the `current` character is an alphabetic letter, the lexer might start building a keyword or identifier token.

- **Tokenization**: The `current` function is also utilized during the tokenization stage. Tokens are formed by grouping sequences of characters that have specific meanings in the language. The `current` function helps in identifying these sequences by allowing the lexer to inspect each character individually.

In summary, the `current` function is a vital utility within the LexerCore class of the Quantum Language compiler. By providing access to individual characters at the current position, it facilitates both the parsing and tokenization processes, ensuring that the lexer can accurately interpret the input source code.