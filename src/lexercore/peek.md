# `peek` Function

## Overview

The `peek` function is an essential utility method within the LexerCore class of the Quantum Language compiler. It allows developers to inspect characters at a specified position in the source code without altering the lexer's current state or advancing its cursor. This functionality is crucial for lookahead operations during lexical analysis, enabling the lexer to determine the next token based on upcoming characters.

## Parameters and Return Value

- **Parameters**:
  - `offset`: An integer representing the number of positions ahead from the current cursor position that you want to inspect. The default value is `0`, which means inspecting the character at the current cursor position.

- **Return Value**:
  - Returns the character located at the specified position (`pos + offset`) in the source code as a `char`. If the specified position exceeds the bounds of the source code, it returns the null character (`'\0'`).

## Edge Cases

1. **Offset Out of Bounds**: When the specified offset results in a position beyond the end of the source code, the function safely returns the null character. This prevents out-of-bounds access errors and ensures that the lexer can handle unexpected input gracefully.

2. **Empty Source Code**: If the source code is empty (`src.size() == 0`), calling `peek()` will always return the null character. This behavior is consistent and predictable, allowing the lexer to initialize correctly even when there is no input to process.

3. **Negative Offset**: While negative offsets are not explicitly handled in the provided code snippet, they would typically be treated as invalid inputs. The lexer should validate such inputs and handle them appropriately, possibly by throwing an exception or logging an error message.

## Interactions with Other Components

The `peek` function interacts closely with the LexerCore class, which manages the overall state of the lexical analyzer. By providing a way to inspect characters without advancing the cursor, `peek` facilitates the implementation of complex lookahead rules required for accurate tokenization. For example, the lexer might use `peek(1)` to check the next character after the current one, aiding in decisions like whether to treat the current character as part of a keyword or identifier.

Additionally, `peek` may interact with other methods within the LexerCore class, such as `advance()`, which moves the cursor forward through the source code. The combination of these functions allows the lexer to perform both forward and backward scanning, which is necessary for certain parsing techniques and error recovery mechanisms.

In summary, the `peek` function is a vital tool for the Quantum Language compiler's lexical analysis phase, enabling safe and efficient inspection of characters in the source code. Its interaction with other components ensures robust and accurate tokenization, supporting the broader goals of the compiler.