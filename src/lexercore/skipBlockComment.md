# skipBlockComment

The `skipBlockComment` function is designed to handle the skipping of block comments in the source code during the lexical analysis phase of the Quantum Language compiler.

## What It Does

This function consumes characters from the input source code stream (`src`) until it encounters the end of a block comment, which is marked by the sequence "*/". Once the end of the block comment is found, the function returns, effectively skipping over the entire block comment without further processing.

## Why It Works This Way

The function operates under the assumption that the current position (`pos`) in the source code stream has already consumed the opening sequence of a block comment, i.e., "/*". The function then enters a loop where it continuously advances through the source code until it finds the closing sequence "*/". During each iteration of the loop, the function checks if the current character is "*" and the next character is "/". If both conditions are met, it skips these two characters and exits the loop, returning control to the caller. If not, it simply advances to the next character in the stream.

This approach ensures that the function correctly identifies and skips block comments, even if they span multiple lines or contain nested comments. By consuming characters one at a time and checking for the closing sequence, the function can accurately determine when to stop.

## Parameters/Return Value

- **Parameters**:
  - None

- **Return Value**:
  - `void`: The function does not return any value; it simply advances the position in the source code stream until it reaches the end of the block comment.

## Edge Cases

1. **Unterminated Block Comment**: If the function encounters an unterminated block comment (i.e., it reaches the end of the source code stream without finding "*/"), it will silently reach the end of the file. This behavior might be considered unexpected, but it simplifies error handling in the lexer by avoiding explicit checks for comment termination.

2. **Nested Comments**: Although the function does not explicitly support nested comments, its design allows it to handle such scenarios gracefully. As long as the closing sequence "*/" appears after the opening sequence "/*", the function will correctly identify and skip the entire block comment.

3. **Comments Spanning Multiple Lines**: The function can easily handle block comments that span multiple lines, as it continues to consume characters until it finds the closing sequence.

## Interactions With Other Components

The `skipBlockComment` function interacts primarily with the following components:

- **LexerCore Class**: This function is part of the `LexerCore` class, which handles the overall lexical analysis process. It is called when the lexer encounters a block comment marker ("/*").

- **Source Code Stream (`src`)**: The function reads characters from the `src` stream, advancing the position (`pos`) as it goes. This interaction is crucial for moving through the source code and identifying comment boundaries.

- **Current Character and Peek Functionality**: The function uses the `current()` method to get the character at the current position and the `peek()` method to look ahead at the next character. These methods are essential for accurately detecting the closing sequence of a block comment.

Overall, the `skipBlockComment` function plays a vital role in the lexer's ability to correctly parse and skip block comments in the source code, ensuring that subsequent tokens are processed accurately.