# skipBlockComment

The `skipBlockComment` function is designed to handle the skipping of block comments in the source code during the lexical analysis phase of the Quantum Language compiler.

## What It Does

This function consumes characters from the input source code stream (`src`) until it encounters the end of a block comment, which is marked by the sequence "*/". Once the end of the block comment is found, the function stops consuming characters and returns, effectively skipping over the entire block comment.

## Why It Works This Way

The function works by iterating through the characters in the source code stream one by one. It checks each character to see if it is followed by another specific character to form part of the block comment delimiter. In this case, it looks for the asterisk (*) followed immediately by the forward slash (/). When both characters are found together, it skips them and returns, thus completing the skipping of the block comment.

This approach ensures that all characters within the block comment are ignored, allowing the lexer to continue processing the rest of the source code without being affected by the commented-out text.

## Parameters/Return Value

- **Parameters**:
  - `src`: A reference to a string representing the source code stream being processed by the lexer.

- **Return Value**:
  - The function has no explicit return value; it simply advances the position in the source code stream until the end of the block comment is reached.

## Edge Cases

1. **Unterminated Block Comment**: If the source code stream ends before encountering the closing "*/" sequence, the function will still return after reaching the end of the stream. This behavior ensures that the lexer can handle incomplete or malformed comments gracefully without throwing an error.

2. **Nested Comments**: The current implementation of `skipBlockComment` does not support nested comments. If a block comment contains another block comment within it, the inner comment will not be properly skipped, leading to potential issues in the lexical analysis process.

3. **Adjacent Comments**: If two block comments are adjacent in the source code without any intervening tokens, the function will consume characters from the first comment until it reaches the second comment's "*/" sequence, then continue from there. This behavior is intentional and allows for the correct handling of multiple consecutive block comments.

## Interactions With Other Components

The `skipBlockComment` function interacts closely with the lexer's state management, particularly the `advance()` method, which moves the lexer's position forward in the source code stream. Additionally, it relies on the `current()` and `peek()` methods to examine the current and next characters in the stream, respectively, to determine when the block comment delimiter has been encountered.

By correctly identifying and skipping block comments, the `skipBlockComment` function enables the lexer to focus on parsing meaningful tokens and constructs, ultimately facilitating accurate compilation of the Quantum Language source code.