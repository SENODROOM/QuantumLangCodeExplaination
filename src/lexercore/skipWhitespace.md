# `skipWhitespace`

## Purpose

The `skipWhitespace` function is designed to consume and skip any whitespace characters in the source code being processed by the Quantum Language compiler's lexer. This includes spaces (' '), tabs ('\t'), and carriage returns ('\r'). The primary purpose of this function is to ensure that the lexer can correctly identify the start of tokens and ignore unnecessary white space during parsing.

## Parameters

- None

## Return Value

- Void (no return value)

## How It Works

The function operates by iterating through the source code string (`src`) starting from the current position (`pos`). During each iteration, it checks if the character at the current position is a whitespace character (either a space, tab, or carriage return). If it encounters a whitespace character, it advances the position pointer (`advance()`), effectively skipping over the whitespace. This process continues until a non-whitespace character is found or the end of the source code is reached.

### Why It Works This Way

This approach ensures that all whitespace characters are consumed before the lexer proceeds to parse tokens. By ignoring these characters, the lexer can focus solely on meaningful content, which simplifies token recognition and reduces complexity in the parsing logic.

## Edge Cases

1. **End of Source Code**: If the end of the source code is reached (i.e., `pos` equals the size of `src`), the loop terminates without attempting to access an invalid index.
2. **Empty Source Code**: If the source code string is empty (`src.size()` is 0), the function will not perform any operations since there are no characters to check.
3. **Mixed Whitespace Characters**: The function handles mixed sequences of whitespace characters (e.g., " \t\r ") efficiently by advancing the position pointer multiple times.

## Interactions With Other Components

The `skipWhitespace` function interacts closely with the lexer's state management. It relies on the `pos` variable to keep track of the current position within the source code string. Additionally, it uses the `current()` method, which likely retrieves the character at the current position (`src[pos]`). After skipping whitespace, the lexer may call other functions such as `matchToken()` to recognize and process the next meaningful token in the source code.