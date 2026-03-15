# skipBlockComment() Function - Multi-Line Comment Skipping

## Overview
The `skipBlockComment()` function is designed to process and discard multi-line comments in the source code that are enclosed between `/*` and `*/`. This function ensures that nested comments within the block are handled correctly, and it also manages scenarios where a block comment might be left unterminated without causing errors.

### Parameters
- None

### Return Value
- The function returns nothing (`void`). It simply skips over the block comment in the source code.

### Edge Cases
1. **Nested Comments**: The function can handle nested comments of the form `/* ... /* ... */ ... */`.
2. **Unterminated Comments**: If a block comment is not properly terminated with `*/`, the function will continue processing until the end of the file (EOF) is reached silently.

### Interactions with Other Components
This function interacts primarily with the Lexer component, which is responsible for breaking down the source code into tokens. By skipping block comments, the Lexer can focus on parsing meaningful tokens rather than commented-out text.

Here's how the function works:

```cpp
{
    // We've already consumed '/*' — skip until '*/'
    while (pos < src.size())
    {
        if (current() == '*' && peek() == '/')
        {
            advance(); // skip *
            advance(); // skip /
            return;
        }
        advance();
    }
    // Unterminated block comment — just silently reach EOF
}
```

### Detailed Explanation
1. **Initial State**: The function assumes that the current character is part of a block comment (i.e., `current()` is `/` and `peek()` is `*`).
2. **Loop Until End of Source Code**: The function enters a loop that continues as long as there are characters remaining in the source code (`pos < src.size()`).
3. **Check for End of Block Comment**:
   - Inside the loop, the function checks if the current character is `*` and the next character is `/` using `current() == '*' && peek() == '/'`.
   - If both conditions are met, it means the end of the block comment has been found. The function then advances twice to consume the `*` and `/` characters and exits the function.
4. **Advance Position**: If the end of the block comment is not yet reached, the function advances the position pointer (`pos`) to move to the next character in the source code.
5. **Handle Unterminated Comments**: If the loop completes without finding the end of the block comment (`*/`), the function will have reached the end of the file. In such cases, it simply returns without any action, effectively ignoring the unterminated comment.

By following these steps, the `skipBlockComment()` function efficiently skips over block comments, ensuring that the Lexer can proceed with parsing the rest of the source code without being distracted by commented-out sections.