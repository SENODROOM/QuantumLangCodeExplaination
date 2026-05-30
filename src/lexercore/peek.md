# `peek` Function

## Overview

The `peek` function is an essential utility method within the LexerCore class of the Quantum Language compiler. It allows developers to inspect characters at a specified position in the source code without altering the lexer's current state or advancing its cursor. This functionality is crucial for lookahead operations during lexical analysis, enabling the lexer to determine the next token based on upcoming characters in the input stream.

## Parameters and Return Value

- **Parameters**:
  - `pos`: The current position of the lexer in the source code.
  - `offset`: The number of positions ahead from the current position that you want to inspect.

- **Return Value**:
  - Returns the character located at the specified position (`pos + offset`) in the source code if it exists; otherwise, returns the null character (`'\0'`).

## How it Works

The `peek` function calculates the target position by adding the `offset` to the current `pos`. If this calculated position is within the bounds of the source code string (`src.size()`), it returns the character at that position. Otherwise, it returns the null character (`'\0'`). This approach ensures that the lexer can safely look ahead without modifying its internal state or causing out-of-bounds errors.

## Edge Cases

1. **Offset Greater than Source Code Length**: When the `offset` exceeds the length of the source code, the function will return the null character (`'\0'`). This prevents any potential runtime errors due to accessing invalid memory locations.
2. **Negative Offset**: Although negative offsets are not common in typical use cases, the function handles them gracefully by returning the character at the current position (`pos`). This means that calling `peek(pos, -1)` will effectively be equivalent to calling `peek(pos)`, which simply returns the current character.

## Interactions with Other Components

The `peek` function interacts closely with the LexerCore class, which manages the overall lexical analysis process. By providing a safe way to inspect characters without advancing the lexer's cursor, it supports various lookahead mechanisms used by the parser to identify tokens and construct the abstract syntax tree (AST). For example, when parsing complex expressions, the lexer might need to look ahead multiple characters to determine the correct token sequence.

In summary, the `peek` function is a vital tool for the Quantum Language compiler's lexical analysis phase. Its ability to inspect characters at a specified position without altering the lexer's state makes it indispensable for implementing lookahead operations, ensuring accurate and efficient tokenization of the source code.