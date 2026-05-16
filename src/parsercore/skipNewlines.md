# `skipNewlines` Function

## Overview

The `skipNewlines` function is an essential component of the parser core within the Quantum Language compiler. Its main objective is to eliminate newline tokens from the input source code during parsing. By doing so, the function facilitates the continuous flow of token processing without interruption due to whitespace characters such as newlines.

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: None

## Edge Cases

1. **Empty Input**: If there are no tokens left in the input source code when `skipNewlines` is called, the function will simply return without performing any operations.
2. **No Newlines**: If the current token is not a newline (`TokenType::NEWLINE`), the function will immediately return, allowing the parser to proceed with the next token.
3. **Consecutive Newlines**: The function can handle consecutive newline tokens gracefully. It will consume each one until a non-newline token is encountered.

## Interactions with Other Components

The `skipNewlines` function interacts closely with the lexer and the parser. During the lexing phase, the lexer generates tokens based on the input source code. When the parser begins its operation, it calls `skipNewlines` to ensure that any initial newlines are removed before proceeding with the actual parsing logic. This interaction is critical as it maintains the integrity of the token stream, preventing premature termination or misinterpretation of subsequent tokens.

In summary, the `skipNewlines` function plays a vital role in preparing the token stream for parsing by removing unnecessary newline tokens. Its implementation is straightforward and efficient, ensuring seamless integration with the rest of the compiler's architecture.