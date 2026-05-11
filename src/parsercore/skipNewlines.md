# `skipNewlines` Function

## Overview

The `skipNewlines` function is a crucial part of the parser core within the Quantum Language compiler. Its primary purpose is to consume and skip any newline tokens encountered in the input source code. This ensures that the parser continues its operation seamlessly, ignoring unnecessary whitespace or line breaks.

## Parameters/Return Value

- **Parameters:** None
- **Return Value:** None

## Edge Cases

1. **No Newline Tokens:** If there are no newline tokens in the input source code, the function will not perform any operations and will simply return.
2. **Multiple Consecutive Newlines:** The function handles multiple consecutive newline tokens gracefully, consuming each one until a non-newline token is encountered.
3. **End of Input:** If the end of the input source code is reached before encountering a non-newline token, the function will stop consuming and return.

## Interactions with Other Components

The `skipNewlines` function interacts closely with the lexer component, which identifies and generates tokens from the input source code. When the lexer encounters a newline character, it generates a `TokenType::NEWLINE` token. The `skipNewlines` function then consumes these tokens, effectively skipping over them during parsing.

Here's how the function integrates into the parser workflow:

1. **Token Consumption Loop:** After generating tokens using the lexer, the parser enters a loop where it processes each token.
2. **Check for Newline Token:** Before processing a token, the parser checks if the current token type is `TokenType::NEWLINE`.
3. **Consume Newline Token:** If a newline token is detected, the `skipNewlines` function is called to consume all consecutive newline tokens.
4. **Continue Parsing:** Once all newline tokens have been consumed, the parser proceeds to process the next token in the sequence.

By ensuring that newline tokens are skipped, the parser maintains a consistent state as it progresses through the input source code, avoiding issues related to unexpected line breaks or formatting inconsistencies.