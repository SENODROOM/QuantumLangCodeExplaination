# `readNumber` Function

## Purpose
The `readNumber` function is designed to parse numeric literals from the source code input of a quantum language compiler. It identifies both integers and floating-point numbers, including those in hexadecimal format. This function ensures that the numeric values are correctly extracted and converted into appropriate token types.

## Parameters
- None

## Return Value
- Returns a `Token` object representing the parsed number. The token type is set to `TokenType::NUMBER`, and the value contains the string representation of the number.

## How It Works
The function operates based on the current character being processed (`current()`) and the next character (`peek()`). Here’s how it works:

1. **Hexadecimal Numbers**:
   - If the current character is `'0'` and the next character is either `'x'` or `'X'`, indicating a hexadecimal number, the function advances twice to skip over `'0x'`.
   - It then enters a loop where it continues advancing as long as the current character is a valid hexadecimal digit (`std::isxdigit(current())`). These digits include `0-9`, `a-f`, and `A-F`.

2. **Decimal Numbers**:
   - For decimal numbers, the function checks if the current character is a digit (`std::isdigit(current())`) or a dot (`.`).
   - If a dot is encountered, the function sets a flag `hasDot` to ensure only one dot appears per number (to distinguish between floating-point numbers and invalid inputs like `123..456`).
   - The function continues advancing characters until it encounters a non-digit or non-dot character.

3. **Suffix Handling**:
   - After parsing the main number part, the function strips any C-style integer or float suffixes such as `'L'`, `'l'`, `'U'`, `'u'`, `'F'`, or `'f'`. These suffixes are consumed but not added to the main number string.

4. **Token Creation**:
   - Finally, the function creates and returns a `Token` object with the type set to `TokenType::NUMBER`, the value containing the parsed number, and the starting line and column positions recorded.

## Edge Cases
- **Empty Input**: If there is no input available at the current position, the function will simply return an empty `Token`.
- **Invalid Hexadecimal Format**: If the input starts with `'0x'` but does not follow with valid hexadecimal digits, the function will stop parsing at the first invalid digit and return the parsed portion.
- **Multiple Dots**: If multiple dots are found within the number, the function will stop parsing and return the number up to the first dot, treating the rest as invalid.

## Interactions With Other Components
- **Lexer Core**: The `readNumber` function is part of the LexerCore class, which is responsible for breaking down the source code into tokens.
- **Tokenizer**: It interacts with the tokenizer to identify and extract numeric literals accurately.
- **Error Handling**: Although not explicitly shown in the provided code snippet, the function should be integrated with error handling mechanisms to manage cases where the input does not conform to expected numeric formats.

This comprehensive approach ensures that the `readNumber` function can effectively handle various numeric formats and edge cases, contributing to the robustness of the quantum language compiler's lexer component.