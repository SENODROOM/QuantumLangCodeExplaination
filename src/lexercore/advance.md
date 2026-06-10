# `advance` Function

## Overview

The `advance` function is a crucial method within the LexerCore class of the Quantum Language compiler. Its primary role is to consume and process the next character from the input source code string (`src`). Each time the function is called, it advances the position pointer (`pos`) by one character and updates the current line number (`line`) and column number (`col`) accordingly.

## Parameters/Return Value

- **Parameters**:
  - None

- **Return Value**:
  - The character that was consumed from the input source code string (`src`).

## Edge Cases

1. **End of String**: If the end of the input string (`src`) is reached, calling `advance` will not increment the position pointer or update the line and column numbers. Instead, it will simply return the null character (`'\0'`), indicating the end of the input.

2. **New Line Character (`'\n'`)**: When encountering a new line character, the function increments both the line number (`line`) and resets the column number (`col`) to 1. This accurately reflects the change in line when moving to the next line in the source code.

3. **Other Characters**: For all other characters encountered, the function only increments the column number (`col`) to reflect the movement to the next character on the same line.

## Interactions with Other Components

- **LexerCore Class**: The `advance` function is a fundamental part of the LexerCore class, which is responsible for lexical analysis (tokenization) of the source code. It interacts directly with the `pos`, `line`, and `col` member variables of the LexerCore instance.

- **Tokenization Process**: During the tokenization process, the LexerCore uses the `advance` function to read through the source code character by character. After advancing, it checks the character against various rules to determine whether it forms part of a valid token, such as identifiers, keywords, operators, or literals.

- **Error Handling**: The `advance` function helps in error handling by providing accurate line and column information when syntax errors occur. This allows for precise error messages that can aid developers in debugging their code.

In summary, the `advance` function plays a vital role in the lexer's operation by consuming characters from the input source code and updating the line and column positions. Its implementation ensures that the lexer can correctly handle different types of characters, including new lines, and provides essential information for error reporting and tokenization.