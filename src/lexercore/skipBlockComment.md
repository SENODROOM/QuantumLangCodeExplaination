# skipBlockComment

The `skipBlockComment` function is designed to handle the skipping of block comments in the source code during the lexical analysis phase of the Quantum Language compiler.

## What It Does

This function consumes characters from the input source code stream (`src`) until it encounters the end of the block comment marker, which is "*/". Once the end marker is found, it advances past both "*" and "/" characters and returns, effectively skipping over the entire block comment.

## Why It Works This Way

The function operates under the assumption that the current position in the source code points to the beginning of a block comment ("/*"). It then enters a loop where it continuously reads characters from the source code until it finds the sequence "*/" indicating the end of the comment. During each iteration, it checks if the current character is "*", followed by "/". If so, it skips these two characters and exits the loop. If not, it simply advances to the next character. This approach ensures that all characters within the block comment are skipped without further processing.

## Parameters/Return Value

- **Parameters**: 
  - `src`: A reference to the string representing the source code being analyzed.
  - `pos`: The current position in the source code stream. This parameter is modified within the function to track progress through the source code.

- **Return Value**:
  - None. The function modifies the `pos` parameter directly and does not return any value.

## Edge Cases

1. **Unterminated Block Comment**: If the source code ends before encountering the "*/" sequence, the function will continue reading characters until the end of the file is reached. In such cases, the function will not throw an error but will simply silently reach the end of the file.
2. **Nested Comments**: The function assumes that there are no nested block comments in the source code. If nested comments are present, the behavior of the function may be unpredictable as it will not correctly identify the end of the outermost comment.

## Interactions With Other Components

The `skipBlockComment` function is typically called when the lexer encounters the start of a block comment ("/*") during its tokenization process. After calling this function, the lexer can safely proceed to the next token without considering the characters within the block comment. This interaction ensures that the lexical analysis phase accurately identifies and processes tokens, excluding those within block comments.