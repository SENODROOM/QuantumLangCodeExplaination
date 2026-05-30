# `readNumber` Function

## Purpose
The `readNumber` function is designed to parse numeric literals from the source code input of a quantum language compiler. It identifies both integers and floating-point numbers, including those in hexadecimal format. This function ensures that the numeric values are correctly recognized and returned as tokens.

## Parameters
- None

## Return Value
- Returns a `Token` object representing the parsed number. The token type is set to `TokenType::NUMBER`, and the token's lexeme contains the numeric string.

## How It Works
1. **Initialization**: The function initializes variables to store the starting line and column positions (`startLine`, `startCol`) and an empty string (`num`) to accumulate the numeric characters. A boolean flag (`hasDot`) is used to track whether a decimal point has been encountered.

2. **Hexadecimal Check**:
   - If the current character is '0' and the next character is either 'x' or 'X', indicating a hexadecimal number, the function advances past these two characters and starts accumulating digits using `std::isxdigit(current())`. This loop continues until a non-hexadecimal character is encountered.

3. **Decimal Number Parsing**:
   - If the current character is not '0' followed by 'x' or 'X', the function assumes it is a decimal number.
   - It enters a loop that continues as long as the current character is a digit or a decimal point. If a decimal point is encountered, it checks if another decimal point has already been found; if so, the loop breaks to avoid parsing invalid floating-point numbers.
   - Each valid character is appended to the `num` string.

4. **Suffix Stripping**:
   - After parsing the main numeric part, the function strips any C-style integer or float suffixes such as 'L', 'l', 'U', 'u', 'F', or 'f'. These suffixes are consumed but not added to the `num` string.

5. **Token Creation**:
   - Finally, the function returns a `Token` object with the type `TokenType::NUMBER`, the accumulated numeric string (`num`), and the original line and column positions.

## Edge Cases
- **Empty Input**: If the input stream is empty or reaches its end before encountering any numeric characters, the function will return a `Token` with an empty lexeme.
- **Invalid Hexadecimal**: If the sequence after '0x' is not valid hexadecimal (i.e., contains non-hexadecimal characters), the function will stop parsing at the first invalid character.
- **Multiple Decimal Points**: If more than one decimal point is encountered in the same number, the function will break out of the loop, resulting in an invalid token.

## Interactions With Other Components
- **Lexer Core**: The `readNumber` function is part of the lexer core, which processes the source code input character by character. It interacts with other functions like `advance()` and `current()` to navigate through the input stream.
- **Tokenization**: By identifying and parsing numeric literals, `readNumber` contributes to the overall tokenization process, ensuring that the compiler can correctly interpret numerical values in the source code.

This comprehensive approach allows the `readNumber` function to handle various numeric formats effectively, enhancing the robustness and accuracy of the quantum language compiler.