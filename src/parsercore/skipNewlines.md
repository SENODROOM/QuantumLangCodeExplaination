# `skipNewlines` Function

## Overview

The `skipNewlines` function is part of the parser core in the Quantum Language compiler. Its primary purpose is to consume and skip any newline tokens encountered during parsing. This ensures that the parser can continue processing the input without being affected by line breaks.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: None

The function does not take any parameters and does not return any value. It operates directly on the current state of the parser, modifying its internal position as needed.

## How It Works

The function uses a loop to repeatedly check if the next token in the input stream is a newline (`TokenType::NEWLINE`). If it is, the function consumes the token using the `consume()` method, effectively advancing the parser's position to the next token. This process continues until a non-newline token is encountered.

Here's a breakdown of the key steps:

1. **Loop Until Non-Newline Token**:
   ```cpp
   while (check(TokenType::NEWLINE))
       consume();
   ```
   - The `while` loop condition checks if the current token type is `NEWLINE`.
   - If the condition is true, the `consume()` method is called to remove the current token from the input stream and advance the parser's position.

2. **Check Method**:
   ```cpp
   bool check(TokenType type) const;
   ```
   - The `check` method takes a `TokenType` parameter and returns `true` if the current token matches the specified type, otherwise it returns `false`.

3. **Consume Method**:
   ```cpp
   void consume() const;
   ```
   - The `consume` method removes the current token from the input stream and advances the parser's position to the next token.

## Edge Cases

- **Empty Input Stream**: If the input stream is empty or contains only newline tokens, the `skipNewlines` function will simply exit after consuming all available newline tokens.
- **Mixed Tokens**: The function will correctly identify and skip newline tokens among other types of tokens. For example, if the input stream contains a mix of newline, identifier, and number tokens, the function will skip only the newline tokens and leave the other tokens intact.

## Interactions With Other Components

- **Lexer**: The `skipNewlines` function relies on the lexer to provide tokens. When the lexer encounters a newline character, it generates a `NEWLINE` token which is then consumed by the parser.
- **Error Handling**: While not explicitly shown in the provided code snippet, the `skipNewlines` function may interact with error handling mechanisms to report issues related to unexpected newlines or malformed input.

In summary, the `skipNewlines` function is crucial for maintaining the flow of parsing by skipping over unnecessary newline tokens. Its implementation leverages the lexer to generate tokens and the parser's own methods to consume them, ensuring robust and accurate parsing of the input stream.