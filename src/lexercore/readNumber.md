# `readNumber` Function

## Purpose
The `readNumber` function is designed to parse numeric literals from the source code input of a quantum language compiler. It identifies both integers and floating-point numbers, including those in hexadecimal format. This function ensures that the numeric values are correctly identified and converted into their respective token types.

## Parameters
- `src`: A reference to the string containing the source code being parsed.
- `pos`: The current position within the source code string.
- `line`: The starting line number of the token being read.
- `col`: The starting column number of the token being read.

## Return Value
- Returns a `Token` object representing the numeric literal parsed from the source code. The token type will be `TokenType::NUMBER`.

## How It Works
The `readNumber` function operates by iterating through the characters in the source code string starting from the current position (`pos`). It determines whether the numeric literal is an integer or a floating-point number based on the first character encountered.

### Parsing Hexadecimal Numbers
If the first character is `'0'` and the next character is either `'x'` or `'X'`, indicating a hexadecimal number, the function proceeds as follows:
1. Advances the position twice to skip over the initial `'0'` and the following `'x'` or `'X'`.
2. Continues advancing the position until it encounters a non-hexadecimal digit (i.e., any character that is not a digit between `0` and `9` or a letter between `A` and `F` or `a` and `f`).
3. Constructs the hexadecimal number string (`num`) by appending each valid hex digit encountered.

### Parsing Decimal Numbers
For decimal numbers, the function follows these steps:
1. Continues advancing the position as long as the current character is either a digit or a period (`.`).
2. If a period is encountered, it sets a flag (`hasDot`) to indicate that a floating-point number is being parsed. If another period is encountered before the end of the iteration, the parsing breaks, assuming the number is invalid.
3. Constructs the decimal number string (`num`) by appending each valid digit or period encountered.

### Handling Suffixes
After constructing the numeric string, the function checks for and consumes optional suffixes that may follow the number:
- `'L'` or `'l'`: Indicates a long long integer.
- `'U'` or `'u'`: Indicates an unsigned integer.
- `'F'` or `'f'`: Indicates a float.

These suffixes are consumed but not added to the numeric string, ensuring they do not interfere with the actual numeric value.

## Edge Cases
- **Empty String**: If the source code string is empty, the function returns a `Token` with an empty string and the current line and column numbers.
- **Invalid Hexadecimal**: If the sequence after `'0x'` is not a valid hexadecimal digit, the function stops reading and returns a `Token` with the partial string and the current line and column numbers.
- **Invalid Floating-Point**: If multiple periods are found in the decimal part of a number, the function stops reading and returns a `Token` with the partial string and the current line and column numbers.
- **No Digits Found**: If no digits or periods are found at the current position, the function returns a `Token` with an empty string and the current line and column numbers.

## Interactions with Other Components
The `readNumber` function interacts with the lexer's state variables such as `line`, `col`, and `pos`. It also uses helper functions like `current()` and `peek()` to access individual characters in the source code string. After parsing a numeric literal, it advances the position past the end of the number and constructs a `Token` object to represent it, which is then returned to the lexer for further processing.

This function is crucial for accurately identifying and handling numeric literals in the source code, ensuring that subsequent stages of the compilation process can work with these values correctly.