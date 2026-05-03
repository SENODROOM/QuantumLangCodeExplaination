# `advance` Function

## Overview

The `advance` function is a critical component of the LexerCore in the Quantum Language compiler. This function processes the source code character by character and updates the lexer's state accordingly. It plays a vital role in tokenization by moving to the next character and updating the current line and column positions.

## Parameters/Return Value

- **Parameters**:
  - None
  
- **Return Value**:
  - A `char`: The next character in the source code.

## Why It Works This Way

The `advance` function increments the position pointer (`pos`) to move to the next character in the source code. If the character read is a newline (`'\n'`), it increments the line counter (`line`) and resets the column counter (`col`) to 1. For all other characters, it simply increments the column counter.

This design ensures that the lexer maintains accurate track of its position within the source code, which is essential for generating meaningful error messages and handling multi-line tokens correctly.

## Edge Cases

- **End of File**: When reaching the end of the file, the function will continue to increment the position pointer until it exceeds the length of the source code. However, since there are no more characters to process, the function will not update the line or column counters further.
  
- **Newline Character**: The function specifically handles the newline character (`'\n'`) to ensure correct line and column tracking. Without this special handling, the lexer would incorrectly assume that the entire source code fits on a single line.

## Interactions With Other Components

- **LexerCore Class**: The `advance` function is part of the `LexerCore` class, which manages the overall lexical analysis process. It interacts with other methods such as `peek`, `match`, and `error` to build up tokens and handle errors respectively.
  
- **Token Generation**: As characters are advanced through the source code, the lexer constructs tokens based on these characters. The `advance` function helps in identifying the boundaries between different tokens, such as spaces, comments, and operators.

- **Error Handling**: In case an unexpected character is encountered, the `advance` function can be used in conjunction with error handling mechanisms to report issues at the correct location in the source code.

By maintaining accurate character positions and updating them appropriately, the `advance` function facilitates efficient and precise tokenization, ensuring that the lexer operates correctly throughout the compilation process.