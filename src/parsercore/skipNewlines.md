# `skipNewlines` Function

## Overview

The `skipNewlines` function is an integral part of the parser core in the Quantum Language compiler. Its primary role is to remove newline tokens from the input source code during the parsing process. This ensures that the parser can handle subsequent tokens seamlessly without being interrupted by line breaks.

## Parameters and Return Value

- **Parameters**: None
- **Return Value**: None

## How It Works

The function operates using a `while` loop that continues as long as the current token type is `TokenType::NEWLINE`. Inside the loop, the `consume()` method is called, which advances the parser's position to the next token in the input stream. This effectively skips over all consecutive newline tokens until a non-newline token is encountered.

### Why It Works This Way

Skipping newlines is crucial because they do not contribute meaningful information to the syntax tree but merely serve as separators between lines of code. By removing these tokens, the parser simplifies its task, focusing only on the relevant syntax elements.

## Edge Cases

1. **Empty Input Stream**: If the input stream is empty or contains only newline characters, the function will simply exit without any action since there are no tokens to consume.
2. **Mixed Token Types**: The function assumes that all tokens are either newline tokens or other significant tokens. If the input stream contains unrecognized token types, the behavior may be unpredictable.
3. **Nested Newlines**: While the function handles consecutive newlines, it does not address nested newlines (e.g., a comment spanning multiple lines). Handling such cases would require additional logic to manage comments correctly.

## Interactions with Other Components

The `skipNewlines` function interacts closely with the lexer, which generates tokens for the parser. Upon encountering a newline token, the lexer passes it to the parser. The parser then uses the `skipNewlines` function to remove all consecutive newline tokens before proceeding with further parsing.

This interaction ensures that the parser receives a continuous stream of tokens without interruptions caused by line breaks, allowing for accurate and efficient parsing of the quantum language source code.