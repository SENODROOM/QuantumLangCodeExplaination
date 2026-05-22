# `skipNewlines` Function

## Overview

The `skipNewlines` function is an essential component within the parser core of the Quantum Language compiler. Its main objective is to eliminate newline tokens from the input source code during the parsing phase. By doing so, the parser is able to proceed seamlessly with handling subsequent tokens without being interrupted by newlines.

### Parameters and Return Value

- **Parameters**: None
- **Return Value**: Void (`void`)

The function does not accept any parameters and does not return any value. It operates directly on the internal state of the parser, modifying its token stream as necessary.

### Edge Cases

1. **Empty Input Stream**: If the input token stream is empty or contains only non-newline tokens, the `skipNewlines` function will have no effect.
2. **No Newlines Present**: The function will iterate through the token stream until it reaches the end, consuming all tokens but leaving none behind if there are no newline tokens present.
3. **Consecutive Newlines**: Multiple consecutive newline tokens will be consumed sequentially, ensuring that only one newline token remains between statements or expressions.

### Interactions with Other Components

The `skipNewlines` function interacts closely with the parser's token consumption mechanism. It relies on two key functions:

1. **`check(TokenType::NEWLINE)`**: This function checks whether the current token in the stream matches the specified `TokenType::NEWLINE`. If it does, the function returns `true`; otherwise, it returns `false`.

2. **`consume()`**: When called, this function removes the current token from the input stream and advances the parser to the next token. The `skipNewlines` function uses `consume()` to discard each newline token it encounters.

By utilizing these functions, `skipNewlines` effectively cleans up the token stream, preparing it for further processing by the parser. This cleanup step is crucial for maintaining the integrity of the parsed structure, especially when dealing with multi-line statements or expressions in the source code.

In summary, the `skipNewlines` function plays a vital role in preprocessing the input source code by removing unnecessary newline tokens, thereby facilitating smoother parsing and reducing complexity in handling multi-line structures.