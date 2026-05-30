# `advance` Function

## Overview

The `advance` function is an essential method within the LexerCore class of the Quantum Language compiler. This function is responsible for consuming and processing the next character from the input source code string (`src`). Upon each invocation, it increments the position pointer (`pos`) to move to the next character in the string. The function also updates the current line number (`line`) and column number (`col`) based on the type of character encountered.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: A `char` representing the next character in the input source code string.

## How It Works

The `advance` function operates as follows:

1. **Consume Character**: It retrieves the character at the current position (`pos`) in the input string (`src`) and assigns it to the variable `c`.
2. **Increment Position**: It then increments the position pointer (`pos`) to move to the next character in the string.
3. **Update Line Number**: If the character retrieved is a newline (`'\n'`), it increments the line number counter (`line`) by one and resets the column number counter (`col`) to one.
4. **Update Column Number**: For any other character, it simply increments the column number counter (`col`) by one.
5. **Return Character**: Finally, it returns the character `c`.

This mechanism ensures that the lexer can accurately track its position within the source code, which is crucial for generating meaningful tokens and error messages during compilation.

## Edge Cases

- **End of String**: When the end of the input string is reached, the `advance` function will continue to increment the position pointer, but since there are no more characters to consume, it will not update the line or column numbers. This behavior allows the lexer to handle the end of the string gracefully without throwing errors.
- **Newline Characters**: Each time a newline character is encountered, the line number is incremented, and the column number is reset to one. This correctly reflects the new line of code being processed.
- **Tab Characters**: Tab characters (`'\t'`) are typically used to indicate indentation in source code. However, in this implementation, tab characters do not affect the column number. Instead, they are treated as single space characters.

## Interactions with Other Components

The `advance` function interacts closely with the LexerCore class's state variables:

- **Position Pointer (`pos`)**: This variable keeps track of the current index in the input string. By advancing it, the lexer moves forward through the source code.
- **Line Number (`line`)**: This variable represents the current line number in the source code. It is updated whenever a newline character is encountered.
- **Column Number (`col`)**: This variable indicates the current column number within the current line. It is updated for all characters except newline characters.

Additionally, the `advance` function may interact with other methods within the LexerCore class, such as `peek`, which checks the next character without advancing the position pointer, and `match`, which advances the position pointer only if the next character matches a specified token.

Overall, the `advance` function plays a fundamental role in the lexer's ability to parse and analyze the input source code, ensuring accurate tracking of character positions and facilitating the generation of tokens and error messages.