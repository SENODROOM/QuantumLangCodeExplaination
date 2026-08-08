# `skipNewlines` Function

## Overview

The `skipNewlines` function is an integral part of the parser core within the Quantum Language compiler. Its primary role is to remove all newline tokens (`TokenType::NEWLINE`) from the current sequence of tokens in the input source code. This process is crucial because newlines can disrupt the flow of parsing and should be disregarded when constructing the abstract syntax tree (AST).

## Parameters/Return Value

- **Parameters**: None
- **Return Value**: None

## How It Works

The function operates through a loop that continues as long as the next token in the sequence is of type `TokenType::NEWLINE`. Within each iteration of the loop, the `consume()` method is called to advance the parser's position to the next token. The `check()` method is used to verify the type of the current token before deciding whether to consume it or not.

### Code Explanation

```cpp
while (check(TokenType::NEWLINE)) {
    consume();
}
```

1. **Loop Condition**: The loop condition `check(TokenType::NEWLINE)` checks if the current token is a newline token. If it is, the loop proceeds; otherwise, it exits.
2. **Consuming Tokens**: Inside the loop, the `consume()` method is invoked. This method updates the internal state of the parser to move to the next token in the sequence.

## Edge Cases

- **Empty Sequence**: If there are no tokens in the sequence, the loop will not execute, and the function will do nothing.
- **No Newlines**: If the sequence does not contain any newline tokens, the loop will also not execute, and the function will terminate without making any changes.

## Interactions with Other Components

The `skipNewlines` function interacts primarily with the lexer component, which generates the sequence of tokens from the input source code. By removing newline tokens, the parser can focus on the meaningful content of the source code, ignoring formatting details like line breaks.

Additionally, this function may interact with other parts of the parser core that rely on a continuous stream of non-newline tokens to construct the AST accurately. Removing newlines ensures that such operations proceed smoothly without interference from formatting elements.

Overall, the `skipNewlines` function plays a vital role in preparing the token sequence for further parsing by eliminating unnecessary formatting details.