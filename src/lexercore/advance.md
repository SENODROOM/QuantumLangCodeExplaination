# `advance` Function

## Overview

The `advance` function is an essential method within the LexerCore class of the Quantum Language compiler. This function is responsible for consuming and processing the next character from the input source code string (`src`). Upon each invocation, it increments the position pointer (`pos`) to move to the subsequent character in the string. Additionally, it updates the current line number (`line`) and column number (`col`) based on the type of character encountered.

## Parameters/Return Value

- **Parameters**:
  - None
  
- **Return Value**:
  - Returns the character that was just consumed as a `char`.

## Why It Works This Way

The `advance` function operates under the principle of lexical analysis, where it breaks down the source code into meaningful tokens. By incrementing the position pointer and updating the line and column numbers, the function ensures accurate tracking of the character stream during tokenization. This is critical for error reporting and maintaining context in complex parsing scenarios.

When a newline character (`'\n'`) is encountered, the function increments the line counter and resets the column counter to 1, reflecting the start of a new line. For all other characters, including spaces, tabs, and alphanumeric characters, the function simply increments the column counter, indicating the progression along the current line.

## Edge Cases

- **End of String**: If the end of the input string (`src`) is reached, the function will continue to return null characters (`'\0'`) until further calls are made. However, this behavior should not be relied upon for normal operation, as it indicates an error or unexpected termination of the input.
  
- **Multi-byte Characters**: The current implementation assumes that the input string consists of single-byte characters. Handling multi-byte characters correctly would require additional logic to ensure proper line and column counting.

## Interactions With Other Components

The `advance` function interacts closely with the LexerCore's state management. It relies on the `pos`, `line`, and `col` member variables to keep track of its progress through the input string. These variables are updated internally within the function to reflect the current character being processed.

Furthermore, the function may interact with other parts of the lexer, such as token recognition functions, which use the current character to determine the type of token being formed. By providing the most recently consumed character, `advance` enables these functions to operate accurately and efficiently.

In summary, the `advance` function plays a pivotal role in the Quantum Language compiler's lexical analysis phase. Through its simple yet effective design, it ensures precise tracking of the character stream, facilitating accurate tokenization and error handling throughout the compilation process.