# skipBlockComment

The `skipBlockComment` function is designed to handle the skipping of block comments in the source code during the lexical analysis phase of the Quantum Language compiler.

## What It Does

This function consumes characters from the input source code stream (`src`) until it encounters the end of a block comment, which is marked by "*/". Once the end of the block comment is found, the function stops consuming characters and returns control back to the caller. If the function reaches the end of the file without encountering the closing "*/" of the block comment, it will silently reach EOF.

## Why It Works This Way

The function operates under the assumption that the lexer has already identified the start of a block comment by consuming the initial "/*". The purpose of `skipBlockComment` is to ensure that all characters within the block comment are skipped over, allowing the lexer to continue processing the rest of the source code after the comment ends. By checking for the sequence "*/", the function can accurately determine when the block comment has concluded, even if the comment spans multiple lines.

## Parameters/Return Value

- **Parameters**:
  - `src`: A reference to the string containing the source code being processed.
  - `pos`: A reference to an integer representing the current position in the source code string. This parameter is modified by the function as it advances through the source code.

- **Return Value**: 
  - The function does not explicitly return any value. However, upon reaching the end of the block comment or EOF, it modifies the `pos` parameter to reflect its new location in the source code.

## Edge Cases

1. **Unterminated Block Comment**: If the function reaches the end of the file without finding the closing "*/" of the block comment, it will simply stop and return, effectively treating the unterminated comment as if it were terminated at EOF. This behavior ensures that the lexer does not get stuck in an infinite loop waiting for a non-existent closing delimiter.

2. **Nested Comments**: The function assumes that there are no nested block comments in the source code. If the source code contains nested comments, the behavior of the lexer may become unpredictable, as the function will only look for the first occurrence of "*/".

3. **Comments Ending on the Same Line**: If the block comment ends on the same line as the initial "/*", the function will still correctly identify the end of the comment and move past it.

## Interactions With Other Components

- **Lexer Core**: `skipBlockComment` is called by the Lexer Core whenever a block comment is encountered. The Lexer Core uses this function to ensure that all characters within the block comment are skipped before continuing with the lexical analysis of the remaining source code.

- **Error Handling**: While `skipBlockComment` itself does not directly handle errors, it contributes indirectly to error handling by ensuring that the lexer does not proceed with invalid or incomplete tokens due to unterminated block comments. If such a situation arises, the lexer will naturally encounter EOF and terminate gracefully, preventing further parsing errors related to incomplete token sequences.