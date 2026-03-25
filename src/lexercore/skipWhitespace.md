# `skipWhitespace`

## Purpose

The `skipWhitespace` function is designed to consume and skip any whitespace characters in the source code being processed by the Quantum Language compiler's lexer. This includes spaces (' '), tabs ('\t'), and carriage returns ('\r').

## Functionality

The function operates within a loop that continues as long as the current position (`pos`) is less than the size of the source code string (`src.size()`), and the character at the current position (`current()`) is one of the whitespace characters specified (space, tab, or carriage return).

- **Loop Condition**: The loop runs until either all characters have been processed (`pos >= src.size()`) or a non-whitespace character is encountered.
- **Character Check**: Inside the loop, the function checks if the current character is a space, tab, or carriage return using the `current()` method.
- **Advance Position**: If the current character is a whitespace, the `advance()` method is called to move the position forward by one character.

## Parameters

- None

## Return Value

- None

## Edge Cases

1. **Empty Source Code**: If the source code string is empty (`src.size() == 0`), the function will not enter the loop and will immediately terminate without performing any operations.
2. **No Whitespace**: If there are no whitespace characters in the source code starting from the current position, the function will exit after checking the first character.
3. **End of String**: If the end of the string is reached during the loop, the function will stop processing further characters.

## Interactions with Other Components

- **Lexer Core**: This function is part of the LexerCore class, which is responsible for breaking down the source code into tokens. By skipping whitespace, it ensures that only meaningful characters are considered when tokenizing the input.
- **Tokenization Process**: As whitespace characters are skipped, the lexer can focus on identifying actual tokens such as keywords, identifiers, operators, and literals more accurately.
- **Position Tracking**: The `pos` variable, which tracks the current position in the source code, is incremented by the `advance()` method each time a whitespace character is skipped. This allows the lexer to continue processing subsequent characters correctly.

In summary, the `skipWhitespace` function plays a crucial role in preparing the source code for tokenization by removing unnecessary whitespace characters, thereby simplifying the parsing process and ensuring accurate token identification.