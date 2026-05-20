# skipBlockComment

The `skipBlockComment` function is designed to handle the skipping of block comments in the source code during the lexical analysis phase of the Quantum Language compiler.

## What It Does

This function consumes characters from the input source code stream (`src`) until it encounters the end of a block comment, which is denoted by "*/". If the end of the file (`EOF`) is reached without encountering "*/", the function simply returns, effectively ignoring any remaining characters as part of an unterminated comment.

## Why It Works This Way

The function operates under the assumption that block comments start with "/*" and end with "*/". By iterating through each character in the source code stream and checking for these sequences, the function can accurately identify and skip over block comments. The use of `advance()` allows the function to move forward one character at a time, ensuring that all characters within the block comment are consumed before returning.

## Parameters/Return Value

- **Parameters**: None
  - The function takes no explicit parameters. It uses member variables or global state to access the current position in the source code stream (`pos`) and the current character being processed (`current()`).

- **Return Value**: Void
  - The function does not return any value. Its primary purpose is to modify the internal state of the lexer by advancing the position in the source code stream until the block comment is fully skipped.

## Edge Cases

1. **Unterminated Block Comment**: If the source code ends without encountering "*/", the function will simply return, leaving the lexer in an undefined state regarding the presence of an unterminated comment. In practice, this situation should be handled gracefully by the parser, which may report an error or ignore the remainder of the input.

2. **Nested Comments**: The function assumes that there are no nested block comments. If a nested comment is encountered, it will continue processing until the first "*/" sequence is found, potentially leading to incorrect parsing of subsequent parts of the source code.

3. **Comments Within Strings**: The function does not account for comments that might appear within string literals. If a string contains "/*" or "*/", it will treat them as part of the string rather than as comment delimiters.

## Interactions With Other Components

- **Lexer Core**: The `skipBlockComment` function is typically called by the lexer core when it encounters a "/*" sequence, indicating the beginning of a block comment. After calling `skipBlockComment`, the lexer can proceed to tokenize the next valid token in the source code.

- **Error Handling**: While the function itself does not handle errors directly, its behavior (silently reaching EOF) can contribute to error reporting in higher-level components like the parser. An unterminated block comment might lead to unexpected tokens or syntax errors later in the compilation process.

In summary, the `skipBlockComment` function is essential for handling block comments in the Quantum Language compiler's lexical analysis phase. By consuming characters until the block comment is terminated, it ensures that the lexer can correctly parse the rest of the source code. However, care must be taken to handle edge cases such as unterminated comments and nested comments appropriately to maintain the integrity of the compilation process.