# skipBlockComment

The `skipBlockComment` function is designed to handle the skipping of block comments in the source code during the lexical analysis phase of the Quantum Language compiler.

## What It Does

This function consumes characters from the input source code stream (`src`) until it encounters the end of a block comment, which is marked by "*/". Once the end of the comment is found, the function returns, effectively skipping over the entire block comment without processing its contents as part of the token stream.

## Why It Works This Way

The function operates under the assumption that block comments begin with "/*" and end with "*/". By iterating through the characters in the source code one by one using the `advance()` method, the function checks for the sequence "*/" at each step. When this sequence is detected, the function skips both the "*" and "/" characters and then exits, ensuring that only valid tokens are processed further in the lexical analysis stage.

## Parameters/Return Value

- **Parameters**:
  - `src`: A reference to the string representing the source code being analyzed.
  - `pos`: The current position within the source code string where the lexer is located.

- **Return Value**: 
  - The function does not explicitly return any value. Instead, it modifies the internal state of the lexer by advancing the position (`pos`) until the end of the block comment is reached.

## Edge Cases

1. **Unterminated Block Comment**: If the source code ends before encountering the "*/" sequence marking the end of a block comment, the function will simply reach the end of the file (`EOF`). In such cases, the lexer may need to handle errors or warnings appropriately, depending on the compiler's design.

2. **Nested Comments**: The function assumes that there are no nested block comments in the source code. If nested comments are allowed, additional logic would be required to correctly identify and skip them.

3. **Comments Within Strings**: The function should be aware of string literals, as they can contain sequences that resemble block comment markers ("*"). Proper handling of these cases ensures that the lexer does not mistakenly interpret strings as comments.

## Interactions With Other Components

- **Lexer Core**: The `skipBlockComment` function is typically invoked by the lexer core when it encounters the start of a block comment ("/*"). After calling `skipBlockComment`, the lexer can continue with the next token or character in the source code.

- **Error Handling**: Depending on the implementation, this function might interact with an error handler component to report unterminated block comments or other issues related to comment parsing.

- **Token Stream**: Although the function itself does not produce tokens, it affects the flow of tokenization by skipping over block comments. This interaction is crucial for maintaining the integrity of the token stream and preventing incorrect parsing of subsequent code.

By carefully managing the consumption of characters and handling edge cases, the `skipBlockComment` function plays a vital role in the lexical analysis process, ensuring that block comments do not interfere with the proper identification and processing of tokens in the Quantum Language compiler.