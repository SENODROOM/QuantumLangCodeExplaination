# skipBlockComment

The `skipBlockComment` function is designed to handle the skipping of block comments in the source code during the lexical analysis phase of the Quantum Language compiler.

## What It Does

This function consumes characters from the input source code stream (`src`) until it encounters the end of a block comment, which is marked by the sequence `*/`. Once the end of the block comment is found, the function returns, effectively skipping over the entire block comment and continuing with the next token in the source code.

## Why It Works This Way

The function operates under the assumption that the current character position (`pos`) has already consumed the opening sequence of a block comment, i.e., `/*`. The function then enters a loop where it continues to consume characters one by one using the `advance()` method. Inside the loop, it checks if the current character is `*` and the next character is `/`, indicating the end of the block comment. If such a sequence is found, the function skips both characters and exits the loop, returning to the caller. If the end of the file (`EOF`) is reached without finding the closing sequence, the function also returns, assuming that the block comment was not properly terminated and reaching the end of the file is acceptable behavior.

## Parameters/Return Value

- **Parameters**:
  - `src`: A reference to the input source code string being analyzed.
  - `pos`: A reference to the current position within the source code string. This parameter is modified as the function consumes characters.

- **Return Value**: None. The function is of type `void`.

## Edge Cases

1. **Properly Terminated Block Comment**: When the block comment ends with `*/`, the function correctly identifies the end and stops consuming characters.
2. **Unterminated Block Comment**: If the block comment is not terminated before the end of the file is reached, the function assumes proper termination and reaches the end of the file.
3. **Nested Comments**: The function does not support nested block comments. If a nested comment is encountered, it will continue to consume characters until the end of the file is reached.

## Interactions With Other Components

The `skipBlockComment` function interacts with the following components:

- **LexerCore Class**: This function is part of the `LexerCore` class, which handles the overall lexical analysis process.
- **Character Access Methods**: The function uses methods like `current()` and `peek()` to access individual characters in the source code string. These methods provide insights into the current and next characters without advancing the position.
- **Advance Method**: The `advance()` method is used to move the current position forward in the source code string, allowing the function to consume characters sequentially.

Overall, the `skipBlockComment` function plays a crucial role in handling block comments during lexical analysis, ensuring that the compiler can proceed with parsing the rest of the source code efficiently.