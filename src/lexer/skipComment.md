# skipComment() Function - Single-Line Comment Skipping

## Overview
The `skipComment()` function is designed to consume and discard single-line comments in the source code that start with `//`. This function ensures that the lexer skips over these comments without including them in the token stream.

## Parameters/Return Value
- **Parameters**: None
- **Return Value**: None

## Why It Works This Way
This implementation of `skipComment()` works by iterating through the source code starting at the current position (`pos`) until it encounters a newline character (`'\n'`). The loop condition checks both whether the end of the source code has been reached (`pos < src.size()`) and whether the current character is not a newline (`current() != '\n'`).

By advancing the position (`advance()`) within the loop, the function effectively consumes each character of the comment one by one. Once a newline character is encountered, the loop terminates, ensuring that only the characters up to the newline are skipped, thus discarding the entire single-line comment.

## Edge Cases
1. **Empty Source Code**: If the source code is empty or consists solely of whitespace, the function will terminate immediately upon entering the loop since `pos < src.size()` will be false.
2. **No Newline in Comment**: If a single-line comment extends to the end of the source code without encountering a newline, the function will continue to iterate indefinitely until it reaches the end of the string. However, due to the check `pos < src.size()`, the function will eventually stop when it reaches the last character of the source code.
3. **Trailing Whitespace**: After skipping a comment, any trailing whitespace on the same line as the comment will also be discarded.

## Interactions With Other Components
The `skipComment()` function interacts primarily with the lexer component. When the lexer encounters a `//` sequence, it calls `skipComment()` to handle the remainder of the line as a comment. After calling `skipComment()`, the lexer advances to the next line, ensuring that subsequent tokens are correctly identified and processed.

Here's how the function might be integrated into the lexer:

```cpp
class Lexer {
public:
    // ... other methods ...

    Token nextToken() {
        // ... other logic ...

        if (current() == '/' && peek() == '/') {
            skipComment();  // Skip the rest of the line as a single-line comment
            return nextToken();  // Advance to the next token after the comment
        }

        // ... other logic ...
    }

private:
    std::string src;  // Source code
    size_t pos = 0;   // Current position in the source code

    char current() const { return src[pos]; }
    char peek() const { return pos + 1 < src.size() ? src[pos + 1] : '\0'; }
    void advance() { ++pos; }

    void skipComment() {
        while (pos < src.size() && current() != '\n')
            advance();
    }
};
```

In this example, the `nextToken()` method first checks if the current character is `/` and the next character is also `/`, indicating the start of a single-line comment. If so, it calls `skipComment()` to skip the rest of the line. After skipping the comment, it recursively calls `nextToken()` to fetch the next valid token from the source code.