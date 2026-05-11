# `current` Function

## Overview

The `current` function is a critical component of the LexerCore class in the Quantum Language compiler. This function retrieves the character currently pointed to by the `pos` cursor within the source code string `src`. If the cursor has reached or surpassed the end of the string, the function returns the null character (`'\0'`) to indicate that there are no more characters to process.

### Parameters and Return Value

- **Parameters**:
  - `pos`: The current position in the source code string `src`.

- **Return Value**:
  - Returns the character at the specified position `pos`.
  - If `pos` is greater than or equal to the size of the source code string `src`, returns the null character (`'\0'`).

### Edge Cases

1. **Position Beyond String Length**: When `pos` equals or exceeds the length of the source code string `src`, the function returns `'\0'`. This prevents out-of-bounds access and ensures that the lexer can handle the end of input gracefully without crashing.

2. **Empty Source Code**: If the source code string `src` is empty, calling `current(0)` will return `'\0'`, indicating that there are no characters available for processing.

### Interactions with Other Components

The `current` function interacts closely with the LexerCore's state management and parsing logic. It is used extensively during the lexical analysis phase to read individual characters from the source code, which are then processed further to identify tokens and build the abstract syntax tree (AST). By providing a safe way to access characters, even when the cursor reaches the end of the string, the `current` function facilitates the robust operation of the lexer throughout the compilation process.