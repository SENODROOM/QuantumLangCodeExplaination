# skipComment

## Description

The `skipComment` function is an essential method within the LexerCore class of the Quantum Language compiler. Its primary role is to traverse through comment sections in the source code during the lexical analysis phase. This allows the compiler to focus solely on parsing significant content rather than being distracted by comments.

## Parameters and Return Value

- **Parameters**: None
- **Return Value**: Void

The function does not take any parameters and does not return any value. It operates directly on the internal state of the LexerCore object, advancing its position until it reaches the end of the current line or encounters a newline character.

## Edge Cases

1. **Single Line Comments**: The function handles single-line comments that start with `//`. It continues reading characters until it finds a newline character (`\n`) or reaches the end of the source code.
2. **Multi-Line Comments**: Although the provided code snippet only addresses single-line comments, the `skipComment` function should be designed to handle multi-line comments as well. Multi-line comments typically start with `/*` and end with `*/`. However, since the implementation focuses on single-line comments, handling multi-line comments would require additional logic.
3. **End of Source Code**: If the function reaches the end of the source code without encountering a newline character, it will stop at the last character of the source code.

## Interactions with Other Components

The `skipComment` function interacts primarily with the LexerCore's internal state, specifically the `pos` variable which tracks the current position in the source code. During each iteration of the loop, the function calls the `advance()` method, which increments the `pos` variable and updates the lexer's internal representation of the current token.

Here is how the function might be extended to handle both single-line and multi-line comments:

```cpp
void LexerCore::skipComment() {
    char firstChar = current();

    if (firstChar == '/') { // Single-line comment
        while (pos < src.size() && current() != '\n')
            advance();
    } else if (firstChar == '*') { // Multi-line comment
        advance(); // Skip the '*' character
        while (pos < src.size()) {
            if (current() == '*' && next() == '\n') {
                advance(2); // Skip the '*' and '\n' characters
                break;
            }
            advance();
        }
    }
}
```

This extended version checks the first character of the potential comment and processes accordingly. For single-line comments, it simply advances until it hits a newline. For multi-line comments, it looks ahead to find the sequence `*/`, skipping over the asterisks and newline when found.