# `readNumber` Function

## Purpose
The `readNumber` function is designed to parse numeric literals from the source code input of a quantum language compiler. It identifies both integers and floating-point numbers, including those in hexadecimal format. This function ensures that the numeric values are correctly recognized and returned as tokens.

## Parameters
- None

## Return Value
- Returns a `Token` object representing the parsed number. The token type is set to `TokenType::NUMBER`, and the token's lexeme contains the numeric string.

## How It Works
The function operates by iterating through the source code starting at the current position (`pos`). It first checks if the current character is '0' followed by either 'x' or 'X', indicating a hexadecimal number. If so, it advances past these characters and continues reading until it encounters a non-hexadecimal digit. For decimal numbers, it reads digits and optionally a single dot ('.') to identify floating-point numbers. After parsing the main number, it strips any common C integer or float suffixes such as 'LL', 'ULL', 'LU', 'L', 'U', 'F', or 'f' (e.g., `1000000007LL`, `3.14f`). Finally, it returns a `Token` object containing the parsed number.

### Detailed Steps:
1. **Initialization**: The function initializes variables `startLine` and `startCol` to store the starting line and column positions of the token. It also creates an empty string `num` to accumulate the numeric characters and a boolean flag `hasDot` to track whether a dot has been encountered.

2. **Hexadecimal Check**:
   - If the current character is '0' and the next character is either 'x' or 'X', it indicates a hexadecimal number.
   - The function advances twice to skip over the '0x' prefix and then enters a loop where it reads all subsequent characters that are valid hexadecimal digits (`std::isxdigit(current())`).

3. **Decimal Number Parsing**:
   - If the current character is not '0' followed by 'x' or 'X', the function assumes it is a decimal number.
   - It enters a loop that continues as long as the current character is either a digit (`std::isdigit(current())`) or a dot ('.').
   - Inside the loop, if a dot is encountered, it checks if a dot has already been found (`hasDot`). If so, it breaks out of the loop to prevent multiple dots in the same number.
   - Each valid character is appended to the `num` string.

4. **Suffix Stripping**:
   - After parsing the main number, the function enters another loop to strip any common C integer or float suffixes.
   - Suffixes include 'L', 'l', 'U', 'u', 'F', and 'f'.
   - These suffixes are consumed without being added to the `num` string.

5. **Return Token**:
   - Once the parsing and suffix stripping are complete, the function returns a `Token` object with the type set to `TokenType::NUMBER`, the lexeme set to the accumulated `num` string, and the starting line and column positions stored in `startLine` and `startCol`.

## Edge Cases
- **Empty Input**: If the input source code is empty, the function will return a `Token` object with an empty lexeme.
- **Invalid Hexadecimal**: If the input starts with '0x' but does not contain valid hexadecimal digits after, the function will return a `Token` object with the '0x' prefix included in the lexeme.
- **Multiple Dots**: If a number contains more than one dot, only the part before the second dot will be considered valid, and the rest will be ignored.
- **Suffixes**: Any unrecognized suffixes following the number will be stripped, ensuring that only the numeric part is returned.

## Interactions With Other Components
- **Lexer Core**: The `readNumber` function is called by the lexer core when it encounters a potential numeric literal in the source code. It processes the numeric literal and returns a corresponding token.
- **Tokenization**: This function plays a crucial role in the tokenization process, which involves breaking down the source code into individual tokens. By accurately parsing numeric literals, it helps maintain the integrity of the token stream for further processing by the parser.

This comprehensive approach ensures that the `readNumber` function can handle various numeric formats and edge cases, making it a robust component of the quantum language compiler's lexer.