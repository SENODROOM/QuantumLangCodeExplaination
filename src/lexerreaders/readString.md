# `readString` Function

## Overview

The `readString` function is designed to lexically analyze and extract string literals from the source code being processed by the Quantum Language compiler's lexer. It reads characters sequentially until it encounters the closing quote, handling escape sequences along the way. The function returns a `Token` object representing the string literal.

## Parameters/Return Value

- **Parameters**:
  - None explicitly defined in the provided snippet; however, it relies on global variables or member functions of the class it belongs to (`line`, `col`, `src`, `pos`, `current()`).

- **Return Value**:
  - A `Token` object of type `STRING`. This token contains the extracted string literal, its starting line number, and starting column number.

## How it Works

1. **Initialization**: 
   - The function records the current line and column numbers where the string starts using `startLine` and `startCol`.
   - It then advances the position pointer `pos` past the opening quote character.

2. **Reading Characters**:
   - The function enters a loop that continues as long as the current position is within bounds (`pos < src.size()`) and the next character is not the closing quote (`current() != quote`).
   - Inside the loop, it checks if the current character is an escape sequence (`'\\'`). If so, it processes the escape sequence accordingly:
     - `\n`: Adds a newline character to the string.
     - `\t`: Adds a tab character to the string.
     - `\r`: Adds a carriage return character to the string.
     - `\\`: Adds a backslash character to the string.
     - `\'`: Adds a single quote character to the string.
     - `\"`: Adds a double quote character to the string.
     - `\0`: Adds a null character to the string.
     - Any other character following a backslash is added verbatim to the string.
   - If the current character is not an escape sequence, it simply appends the character to the string and advances the position pointer.

3. **Handling Edge Cases**:
   - If the loop exits because the end of the source code has been reached (`pos >= src.size()`), the function throws a `QuantumError` indicating an unterminated string literal.
   - If the loop completes successfully, the function advances the position pointer past the closing quote and returns a `Token` object containing the string literal, its starting line and column numbers.

4. **Interactions with Other Components**:
   - The function interacts with the `LexerReaders.cpp` file, which likely contains other lexical analysis functions.
   - It uses global variables such as `line`, `col`, `src`, and `pos` to keep track of the current state of the source code being analyzed.
   - The `current()` function is assumed to be a member function of the class that calls `readString`, returning the current character at `pos`.

## Example Usage

Here’s how you might use the `readString` function in a context:

```cpp
// Assuming 'lexer' is an instance of the Lexer class
int startLine = lexer.line;
int startCol = lexer.col;
std::string sourceCode = "..."; // Source code containing a string literal
lexer.src = sourceCode;
lexer.pos = ...; // Position pointing to the first character of the string literal

try {
    Token token = lexer.readString();
    // Process the token
} catch (const QuantumError& e) {
    // Handle error
}
```

In this example, `lexer.readString()` would attempt to extract a string literal from the `sourceCode` starting at the specified `pos`. If successful, it would return a `Token` object. If an error occurs (e.g., the string is not properly terminated), it would throw a `QuantumError`.

This function is crucial for correctly parsing and interpreting string literals in the source code, ensuring that they are handled appropriately during further stages of compilation.