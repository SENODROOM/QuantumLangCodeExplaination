# `skipNewlines` Function

## Overview
The `skipNewlines` function is a utility method within the Quantum Language compiler's parser component. Its primary role is to consume and ignore any sequence of newline tokens (`TokenType::NEWLINE`) encountered during parsing. This ensures that the parser can proceed without being affected by multiple consecutive newlines in the source code.

## Parameters
- None

## Return Value
- None

## How It Works
The function operates through a loop that continues as long as the current token type is `TokenType::NEWLINE`. Inside the loop:
1. The `check` function is used to verify if the current token type matches `TokenType::NEWLINE`.
2. If a newline token is confirmed, the `consume` function is called to advance the parser to the next token in the input stream.

This process effectively skips over any number of consecutive newline characters, allowing the parser to maintain its state and continue processing subsequent tokens.

## Edge Cases
1. **Empty Input**: If there are no tokens in the input stream, the `check` function will immediately return false, and the loop will not execute.
2. **Non-Newline Tokens**: Once a non-newline token is encountered, the loop terminates, and the parser moves on to handle that token.
3. **Mixed Token Types**: The function will correctly identify and skip only newline tokens, leaving other types of tokens intact.

## Interactions with Other Components
- **Lexer**: The `skipNewlines` function relies on the lexer to generate tokens. When the lexer encounters a newline character, it produces a `TokenType::NEWLINE` token.
- **Parser State**: By consuming newline tokens, the parser updates its internal state to reflect the progress made in skipping whitespace. This is crucial for maintaining accurate line numbers and column positions when reporting errors or generating output.
- **Error Handling**: Although not directly involved in error handling, the correct consumption of newline tokens helps ensure that error messages are associated with the appropriate lines of code, improving debugging experience.

In summary, the `skipNewlines` function is an essential part of the parser's infrastructure, designed to handle whitespace and maintain the integrity of the parsing process by ignoring unnecessary newline characters.