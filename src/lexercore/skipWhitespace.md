# `skipWhitespace`

## Purpose

The `skipWhitespace` function is designed to consume and skip any whitespace characters in the source code being processed by the Quantum Language compiler's lexer. This includes spaces (' '), tabs ('\t'), and carriage returns ('\r'). The primary purpose of this function is to ensure that the lexer can correctly identify tokens without being affected by unnecessary whitespace.

## Parameters/Return Value

- **Parameters**: None
  - The function operates on the internal state of the lexer, specifically the `pos` variable which represents the current position in the source code string (`src`).

- **Return Value**: None
  - The function modifies the internal state of the lexer but does not return any value explicitly.

## How It Works

The function uses a `while` loop to iterate through the source code starting from the current position (`pos`). Inside the loop, it checks if the character at the current position (`current()`) is one of the whitespace characters: space (' '), tab ('\t'), or carriage return ('\r'). If the condition is true, the function calls `advance()` to move the position forward by one character. This process continues until either the end of the source code is reached (`pos >= src.size()`) or a non-whitespace character is encountered.

### Why It Works This Way

This approach ensures that all consecutive whitespace characters are skipped efficiently. By using a loop and checking each character individually, the function can handle multiple consecutive whitespace characters without requiring additional logic to manage the count. The use of `advance()` simplifies the process of moving the position forward, making the implementation straightforward and easy to understand.

## Edge Cases

1. **Empty Source Code**: If the source code string (`src`) is empty, the function will immediately exit the loop as `pos` will be greater than or equal to `src.size()`, effectively skipping over any potential whitespace characters.

2. **No Whitespace Characters**: If there are no whitespace characters in the source code starting from the current position, the function will also exit the loop immediately once it encounters the first non-whitespace character.

3. **End of Line Character**: Carriage return ('\r') is considered a whitespace character, so the function will skip these characters as well. However, it's important to note that the lexer should also handle line termination properly, typically by recognizing newline characters ('\n') and possibly other line terminators depending on the language specification.

## Interactions With Other Components

The `skipWhitespace` function interacts closely with the lexer's internal state management, particularly the `pos` variable and the `current()` method. These variables and methods are essential for tracking the current position in the source code and retrieving the character at that position, respectively. By consuming whitespace characters, the lexer ensures that subsequent tokenization processes are accurate and do not misinterpret whitespace as part of a token. Additionally, `skipWhitespace` might be called before or after other lexical analysis functions to maintain the correct parsing context.