# skipComment

The `skipComment` function is designed to handle comments in the source code being processed by the Quantum Language compiler. Comments are sections of code that are ignored by the compiler and serve as notes or explanations for the programmer.

## What It Does

The primary purpose of the `skipComment` function is to skip over comment lines in the input source code. This allows the lexer to continue processing the next valid token after encountering a comment.

### Why It Works This Way

The function uses a loop to iterate through the source code starting from the current position (`pos`). It continues advancing the position until it encounters a newline character (`'\n'`) or reaches the end of the source code (`src.size()`). This ensures that the entire comment line is skipped, including any characters that might be part of the comment syntax.

### Parameters/Return Value

- **Parameters**:
  - None

- **Return Value**:
  - The function returns `void`, meaning it doesn't return any value. It simply advances the position in the source code to skip over the comment.

### Edge Cases

1. **End of Source Code**: If the current position (`pos`) is already at the end of the source code (`src.size()`), the loop condition will fail immediately, and the function will not perform any action.
2. **Single Line Comment**: The function assumes that single-line comments start with `//`. If the comment starts with something else (e.g., `/*` for multi-line comments), the function may not work correctly.
3. **Trailing Characters**: If there are characters following the comment on the same line, these characters will also be skipped until the newline character is encountered.

### Interactions With Other Components

The `skipComment` function interacts with the lexer's state machine by advancing the position (`pos`) within the source code. This advancement occurs inside the loop, which continues until either the end of the source code or a newline character is reached. Once the loop exits, the lexer can proceed to process the next token in the source code.

Here is the updated implementation of the `skipComment` function:

```cpp
void LexerCore::skipComment() {
    while (pos < src.size() && current() != '\n') {
        advance();
    }
}
```

This function effectively skips over comment lines, ensuring that the lexer processes only valid tokens.