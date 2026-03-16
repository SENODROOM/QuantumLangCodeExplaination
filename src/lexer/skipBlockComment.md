# skipBlockComment() Function - Multi-Line Comment Skipping

## Overview
The `skipBlockComment()` function processes and discards multi-line comments in the source code that are enclosed between `/*` and `*/`. This function ensures that nested comments within the block are handled correctly, preventing premature termination of the comment block.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: None

## How It Works
The function starts by assuming that the current position (`pos`) has already consumed the opening sequence `/*`. It then enters a loop that continues until the end of the source code is reached (`pos < src.size()`). Inside the loop:

1. The function checks if the current character (`current()`) is an asterisk (`*`) and the next character (`peek()`) is a forward slash (`/`). If both characters match, it indicates the end of the comment block (`*/`).
2. If the end of the comment block is detected, the function advances twice to consume the asterisk and the forward slash, effectively skipping the entire comment block.
3. If the end of the comment block is not detected, the function simply advances once to move to the next character in the source code.

This approach ensures that even if there are nested comments within the block, the function will continue to search for the closing sequence `*/`, thereby correctly terminating the comment block.

## Edge Cases
- **Unterminated Block Comment**: If the source code ends without encountering the closing sequence `*/`, the function will silently reach the end of the file (EOF) without throwing any errors or warnings. This behavior is intentional and allows the compiler to handle incomplete comments gracefully.
- **Nested Comments**: The function can handle nested comments because it continues searching for the closing sequence `*/` regardless of whether it encounters another `/*` before reaching the end of the block.

## Interactions With Other Components
The `skipBlockComment()` function interacts with the lexer component of the Quantum Language compiler. It is typically called when the lexer encounters the opening sequence `/*` during tokenization. After skipping the comment block, the lexer can resume processing the remaining tokens in the source code.

In summary, the `skipBlockComment()` function efficiently skips over multi-line comments in the source code, ensuring correct handling of nested comments and graceful management of unterminated blocks.