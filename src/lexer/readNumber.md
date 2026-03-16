# readNumber Function - Numeric Literal Tokenization

## Overview
The `readNumber()` function is a comprehensive numeric literal parser designed to recognize and tokenize various number formats in source code, including decimal numbers, hexadecimal literals, floating-point numbers, and C-style type suffixes such as `LL`, `ULL`, `LU`, `L`, `U`, `F`, and `f`.

### Why It Works This Way
This implementation ensures that all possible numeric formats are correctly identified and parsed:
- **Decimal Numbers**: Recognized by default if the first character is not `0` followed by `x` or `X`.
- **Hexadecimal Literals**: Identified when the first two characters are `0x` or `0X`. The parser continues to read until a non-hexadecimal digit is encountered.
- **Floating-Point Numbers**: Recognized when a dot (`.`) is present among digits. If multiple dots are found, the parsing breaks.
- **C-Style Type Suffixes**: Consumed without adding them to the token's value, ensuring that only the numeric part is returned.

## Parameters/Return Value
- **Parameters**:
  - None explicitly defined within the function signature; it operates on global variables `line`, `col`, `pos`, and `src` which represent the current line number, column number, position in the source code string, and the source code itself, respectively.
  
- **Return Value**:
  - A `Token` object of type `NUMBER` containing the numeric literal as its value, along with the starting line and column numbers.

## Edge Cases
- **Leading Zero**: Decimal numbers cannot have leading zeros unless they are zero itself (e.g., `0`).
- **Multiple Dots**: Only one dot can be present in a floating-point number. Multiple dots will cause the parser to stop reading at the second dot.
- **Empty Input**: If the input stream is empty or does not contain any valid numeric literal, the function will return an empty token.
- **Invalid Hexadecimal Characters**: Any character that is not a valid hexadecimal digit (`0-9`, `a-f`, `A-F`) after `0x` or `0X` will terminate the hexadecimal literal.
- **Type Suffixes**: Suffixes like `LL`, `ULL`, `LU`, `L`, `U`, `F`, and `f` are ignored during tokenization but must follow immediately after the numeric literal.

## Interactions With Other Components
- **Lexer Class**: This function is part of the Lexer class, which is responsible for breaking down the source code into individual tokens. The `readNumber()` function is called when the lexer encounters a potential numeric literal.
- **Global Variables**: The function relies on several global variables:
  - `line`: Tracks the current line number in the source code.
  - `col`: Tracks the current column number in the source code.
  - `pos`: Keeps track of the current position in the source code string.
  - `src`: Holds the entire source code string being processed.
- **Helper Functions**: Uses helper functions like `advance()`, `current()`, and `peek()` to navigate through the source code string and check the next character(s).

By carefully handling different numeric formats and ignoring unnecessary type suffixes, the `readNumber()` function ensures accurate tokenization of numeric literals in the source code.