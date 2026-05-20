# `readNumber` Function

## Purpose
The `readNumber` function is designed to parse numeric literals from the source code input of a quantum language compiler. It identifies both integers and floating-point numbers, including those in hexadecimal format. This function ensures that the numeric values are correctly recognized and returned as tokens.

## Parameters
- `src`: A string representing the source code input being parsed.
- `line`: An integer indicating the starting line number of the token being read.
- `col`: An integer indicating the starting column number of the token being read.

## Return Value
The function returns a `Token` object of type `TokenType::NUMBER`, containing the numeric literal as its lexeme, along with the starting line and column numbers.

## How It Works
The `readNumber` function operates by iterating through the characters of the source code input (`src`) starting from the current position (`pos`). Here’s how it handles different types of numeric literals:

1. **Hexadecimal Numbers**:
   - If the current character is `'0'` and the next character is either `'x'` or `'X'`, it recognizes the beginning of a hexadecimal number.
   - The function advances the position twice to skip over the initial `'0x'`.
   - It then continues advancing until it encounters a non-hexadecimal digit (using `std::isxdigit(current())`).

2. **Decimal Numbers**:
   - For decimal numbers, the function checks if the current character is a digit or a period (`.`).
   - If a period is encountered, it sets a flag (`hasDot`) to indicate that a floating-point number might be present.
   - The function continues advancing until it encounters a non-digit or non-period character.

3. **Integer/Float Suffixes**:
   - After parsing the main numeric part, the function strips any trailing integer or float suffixes such as `'L'`, `'l'`, `'U'`, `'u'`, `'F'`, or `'f'`. These suffixes are common in C-style languages and do not affect the numeric value itself.

## Edge Cases
- **Empty Input**: If the input string is empty, the function will return an invalid token.
- **Invalid Hexadecimal Format**: If the input starts with `'0x'` but contains non-hexadecimal digits after that, the function will stop at the first invalid digit.
- **Trailing Suffixes**: Any trailing suffixes like `'LL'`, `'ULL'`, etc., will be ignored, ensuring only the numeric value is captured.

## Interactions With Other Components
- **Lexer Core**: The `readNumber` function is part of the lexer core component responsible for breaking down the source code into individual tokens.
- **Error Handling**: While not explicitly shown in the provided code snippet, the function should integrate with error handling mechanisms to report issues like invalid numeric formats or unexpected characters.

This function is crucial for accurately interpreting numeric literals in the source code, which can then be used by subsequent stages of the compiler for further processing.