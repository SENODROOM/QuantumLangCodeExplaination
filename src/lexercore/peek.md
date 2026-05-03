# `peek` Function

## Overview

The `peek` function is an essential utility method within the LexerCore class of the Quantum Language compiler. It allows developers to inspect characters at a particular position in the source code without altering the lexer's current state or advancing its position pointer. This functionality is crucial for lookahead operations during lexical analysis, enabling the parser to make informed decisions based on upcoming tokens.

## Parameters and Return Value

- **Parameters**:
  - `offset`: A `size_t` type parameter that specifies the number of positions ahead from the current lexer position (`pos`) where the character should be examined. If `offset` is zero, the function returns the character at the current lexer position.

- **Return Value**:
  - The function returns a `char` representing the character located at the specified position (`pos + offset`). If the calculated position exceeds the bounds of the source code string (`src.size()`), the function returns the null character (`'\0'`).

## Edge Cases

1. **Offset Greater Than Source Size**: When the provided `offset` is greater than the length of the source code string, the function correctly handles this by returning the null character. This prevents out-of-bounds access and ensures robustness against invalid input offsets.

2. **Current Position at End**: If the current lexer position (`pos`) is already at the end of the source code string, attempting to peek further will result in the null character being returned. This behavior is consistent with the expected outcome of looking ahead beyond the last character.

3. **Negative Offset**: Although not explicitly handled in the given code snippet, negative offsets can be considered as valid inputs in some contexts. However, since the implementation uses addition (`p = pos + offset`), negative offsets would effectively move the position backward within the source code string. In practical scenarios, handling negative offsets might involve additional logic to ensure they do not cause the lexer to go past the beginning of the string.

## Interactions with Other Components

The `peek` function interacts closely with the LexerCore class, which manages the overall state of the lexer including the current position (`pos`) and the source code string (`src`). By providing a means to look ahead without advancing the position, the `peek` function facilitates more complex parsing algorithms that require context-aware tokenization.

Here’s how `peek` might interact with other parts of the Lexer:

- **Token Recognition**: During token recognition, the lexer needs to determine the type of the next token based on the sequence of characters. The `peek` function allows the lexer to examine multiple characters ahead, aiding in the identification of multi-character tokens such as operators or keywords.

- **Error Handling**: In error handling mechanisms, the lexer may need to backtrack or skip over certain characters to recover from errors. The `peek` function enables the lexer to safely check characters without affecting the ongoing parsing process.

- **Lookahead Parsing Algorithms**: Advanced parsing algorithms often rely on lookahead to decide the next action. For example, recursive descent parsers use lookahead to predict whether a non-terminal rule can be matched starting from the current position. The `peek` function provides the necessary support for these algorithms by allowing them to inspect future characters.

In summary, the `peek` function is a vital component of the LexerCore class, offering a safe and efficient way to perform lookahead operations during lexical analysis. Its interaction with other parts of the lexer supports more sophisticated parsing techniques, ensuring accurate and robust language processing.