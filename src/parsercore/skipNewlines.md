# `skipNewlines` Function

## Overview

The `skipNewlines` function is an essential component of the parser core in the Quantum Language compiler. Its main purpose is to eliminate all newline tokens (`TokenType::NEWLINE`) from the current sequence of tokens in the input source code. This ensures that the subsequent parsing process can focus on meaningful language constructs rather than whitespace or formatting issues.

## Parameters and Return Value

- **Parameters**: None
- **Return Value**: void

The function does not take any parameters and does not return any value. It operates directly on the internal state of the parser, modifying the token stream as necessary.

## How It Works

The `skipNewlines` function uses a loop to repeatedly check if the next token in the stream is of type `TokenType::NEWLINE`. If it is, the function consumes that token using the `consume()` method. This process continues until there are no more newline tokens left at the beginning of the stream.

### Code Explanation

```cpp
void ParserCore::skipNewlines() {
    while (check(TokenType::NEWLINE)) {
        consume();
    }
}
```

1. **Loop Condition**: The `while` loop checks whether the current token is a newline token using the `check()` method.
2. **Consuming Tokens**: Inside the loop, the `consume()` method is called to remove the current token from the stream.
3. **Continuation**: The loop repeats until no more newline tokens are found at the start of the stream.

## Why It Works This Way

This approach ensures that all leading newlines are removed efficiently. By checking each token individually and consuming them immediately, the function avoids unnecessary overhead and maintains a clean token stream for further parsing operations. The use of a loop allows for repeated consumption of consecutive newline tokens without requiring multiple calls to the function.

## Edge Cases

- **Empty Token Stream**: If the token stream is empty or contains no newline tokens, the function will terminate immediately without performing any actions.
- **Non-Newline Tokens at Start**: The function only removes newline tokens from the start of the stream. If non-newline tokens precede newline tokens, they will remain untouched.

## Interactions with Other Components

The `skipNewlines` function interacts primarily with the token stream managed by the parser core. It relies on the `check()` and `consume()` methods to interact with the token stream. These methods are part of the parser's infrastructure and ensure that the token stream is correctly updated during the parsing process.

By removing newline tokens, the `skipNewlines` function facilitates a smoother interaction between different parts of the parser, such as the lexer and the syntax analyzer, by ensuring that the token stream starts with valid language constructs. This contributes to the overall robustness and reliability of the Quantum Language compiler.