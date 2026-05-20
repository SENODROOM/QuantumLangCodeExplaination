# `advance` Function

## Overview

The `advance` function is a crucial component of the LexerCore in the Quantum Language compiler. Its main purpose is to consume and process the next character from the input source code string (`src`). As characters are processed, the function updates the lexer's internal state, including the current line number (`line`) and column position (`col`). This ensures that accurate error reporting and location tracking can be provided during the compilation process.

## Parameters

- **None**: The `advance` function operates on the internal state of the LexerCore and does not take any explicit parameters.

## Return Value

- **char**: The function returns the character that was just consumed from the input source code. This allows the caller to inspect the character that has been processed.

## Edge Cases

- **End of Input**: If the end of the input source code is reached, the function will continue to increment the line and column positions accordingly. However, since there are no more characters to consume, the returned character will be the null terminator (`'\0'`).
- **New Line Character**: When encountering a new line character (`'\n'`), the function increments the line counter and resets the column counter to 1. This accurately reflects the start of a new line in the source code.

## Interactions with Other Components

- **LexerCore State**: The `advance` function modifies the internal state of the LexerCore, specifically the `pos`, `line`, and `col` variables. These changes are used by other functions within the LexerCore to correctly track the position of the lexer as it processes the source code.
- **Tokenization**: By consuming characters one by one, the `advance` function contributes to the overall tokenization process. It helps in identifying tokens such as keywords, identifiers, literals, and operators by checking the sequence of characters and updating its state based on the encountered symbols.

In summary, the `advance` function plays a vital role in the LexerCore by processing the source code one character at a time, updating the lexer's internal state, and facilitating accurate tokenization. Its design ensures that the lexer remains synchronized with the input source code, enabling precise error handling and location tracking throughout the compilation process.