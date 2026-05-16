# `readNumber` Function

## Purpose
The `readNumber` function is designed to parse numeric literals from the source code input of a quantum language compiler. It identifies both integers and floating-point numbers, including those in hexadecimal format. This function ensures that the numeric values are correctly identified and tokenized as `NUMBER`.

## Parameters
- None

## Return Value
- Returns a `Token` object of type `NUMBER`, containing the parsed numeric literal as its lexeme, along with the starting line and column positions within the source code.

## How It Works
The function operates by iterating through the source code string (`src`) starting from the current position (`pos`). It checks the first character to determine whether the number is in hexadecimal format or not:

1. **Hexadecimal Numbers**: If the first character is '0' and the next character is either 'x' or 'X', indicating a hexadecimal number, the function reads two characters ('0x') into the `num` string. It then continues to read subsequent characters until it encounters a non-hexadecimal digit. The resulting string is returned as a `NUMBER` token.

2. **Decimal Numbers**: For decimal numbers, the function enters a loop where it reads digits and possibly a decimal point ('.') into the `num` string. If a decimal point is encountered, the function checks if there is already a decimal point present in the `num`. If so, the loop breaks, as multiple decimal points are invalid. After reading all valid digits and the decimal point, the function strips any common C integer or float suffixes such as 'L', 'l', 'U', 'u', 'F', or 'f'. These suffixes are consumed but not added to the `num` string. Finally, the function returns a `NUMBER` token containing the parsed numeric literal.

## Edge Cases
- **Invalid Hexadecimal Format**: If the initial '0x' sequence is followed by non-hexadecimal digits, the function will stop reading at the first invalid digit and return the valid part as a `NUMBER` token.
- **Multiple Decimal Points**: If a numeric literal contains more than one decimal point, the function will stop reading after the second decimal point and return the valid part up to that point as a `NUMBER` token.
- **Suffix Stripping**: The function correctly handles various C-style suffixes for integers and floats, ensuring that they do not interfere with the numeric value being parsed.

## Interactions With Other Components
- **Lexer Core**: The `readNumber` function is part of the LexerCore class, which is responsible for breaking down the source code into tokens. It interacts with the Lexer's state variables like `line`, `col`, and `pos` to track the location of the numeric literal within the source code.
- **Token Class**: The function creates and returns a `Token` object, which encapsulates the type of token (`NUMBER`), the lexeme (the parsed numeric literal), and the starting line and column positions. This token is then used by higher-level parsing components to construct the abstract syntax tree (AST).

This comprehensive approach ensures that the `readNumber` function can accurately parse a wide range of numeric literals from the source code, making it a crucial component of the lexer in a quantum language compiler.