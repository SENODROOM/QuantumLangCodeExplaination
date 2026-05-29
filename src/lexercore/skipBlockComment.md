# skipBlockComment

The `skipBlockComment` function is designed to handle the skipping of block comments in the source code during the lexical analysis phase of the Quantum Language compiler.

## What It Does

This function consumes characters from the input source code stream (`src`) until it encounters the end of a block comment, which is marked by the sequence "*/". Once the end of the block comment is found, the function returns, effectively skipping over the entire block comment.

## Why It Works This Way

The function operates under the assumption that block comments start with "/*" and end with "*/". It uses a loop to iterate through each character in the source code stream. Inside the loop, it checks if the current character is "*" and the next character is "/". If both conditions are met, it means the end of the block comment has been reached, and the function skips these two characters before returning. If the end of the block comment is not found within the stream, the function simply advances to the next character and continues the loop until the end of the file (EOF) is reached.

## Parameters/Return Value

- **Parameters**: 
  - `src`: A string representing the source code being analyzed.
  - `pos`: An integer representing the current position in the source code stream. The function modifies this parameter as it iterates through the source code.

- **Return Value**:
  - The function does not explicitly return any value. Instead, it modifies the `pos` parameter to indicate the new position after skipping the block comment.

## Edge Cases

1. **Unterminated Block Comment**: If the function reaches the end of the file without encountering the "*/" sequence marking the end of the block comment, it will simply return, leaving the `pos` at the EOF. This behavior ensures that the lexer can continue processing the rest of the source code even if there is an unterminated block comment.

2. **Nested Comments**: The function assumes that block comments cannot be nested. If the source code contains nested block comments, the function may not behave correctly, potentially leading to premature termination or failure to skip the correct portion of the code.

## Interactions With Other Components

The `skipBlockComment` function interacts with the following components:

- **LexerCore Class**: This function is part of the `LexerCore` class, which handles the overall lexical analysis process of the source code.

- **Character Access Methods**: The function uses methods like `current()` and `peek()` to access individual characters in the source code stream. These methods help in identifying the characters necessary to determine the end of the block comment.

- **Advance Method**: The function calls the `advance()` method to move to the next character in the source code stream. This method updates the `pos` parameter accordingly.

In summary, the `skipBlockComment` function efficiently skips over block comments in the source code, ensuring that the lexer can proceed without interruption. Its design accounts for potential edge cases such as unterminated comments and non-nested structures, making it robust for various inputs.