# `readNumber` Function

## Purpose
The `readNumber` function is designed to parse numeric literals from the source code input of a quantum language compiler. It identifies both integers and floating-point numbers, including those in hexadecimal format. This function ensures that the numeric values are correctly recognized and converted into appropriate token types.

## Parameters
- None

## Return Value
- Returns a `Token` object representing the parsed numeric literal. The `TokenType` is set to `NUMBER`, and the `value` contains the string representation of the number.

## How it Works
The function operates as follows:
1. **Initialization**: It initializes variables `startLine` and `startCol` to store the starting line and column positions of the number being read. It also initializes an empty string `num` to accumulate the characters forming the number and a boolean flag `hasDot` to track whether a decimal point has been encountered.

2. **Hexadecimal Check**:
   - If the current character (`current()`) is '0' and the next character (`peek()`) is either 'x' or 'X', indicating a hexadecimal number, the function reads the '0x' prefix and continues reading subsequent characters until it encounters a non-hexadecimal digit.
   - Each valid hexadecimal digit (0-9, A-F, a-f) is appended to the `num` string.

3. **Decimal Number Reading**:
   - If the current character is not '0' followed by 'x' or 'X', the function assumes it's a decimal number.
   - It reads digits and optionally a single decimal point ('.') from the source code.
   - If more than one decimal point is found, the loop breaks, preventing further parsing of the fractional part.

4. **Suffix Handling**:
   - After reading the main number, the function checks for any C-style integer or float suffixes such as 'L', 'l', 'U', 'u', 'F', or 'f'.
   - These suffixes are consumed but not added to the `num` string, ensuring that only the numeric part is returned as a token.

5. **Token Creation**:
   - Finally, the function returns a `Token` object with the type set to `NUMBER`, the value containing the parsed number, and the original starting line and column positions.

## Edge Cases
- **Empty Input**: If there are no characters left to read, the function will return an empty token.
- **Invalid Hexadecimal**: If the sequence starts with '0x' but contains invalid hexadecimal digits, the function will stop at the first invalid digit.
- **Multiple Decimal Points**: Parsing stops after encountering more than one decimal point, treating everything before it as a valid number.
- **Suffixes**: Suffixes like 'L', 'l', 'U', 'u', 'F', or 'f' are ignored, allowing the function to handle numbers without these additional specifications.

## Interactions with Other Components
- **Lexer**: The `readNumber` function is called within the lexer component when a potential number is detected. It processes the input stream and determines if the detected sequence is indeed a number.
- **Tokenization**: Once a number is successfully parsed, it is encapsulated into a `Token` object and passed on to the parser for further processing.
- **Error Handling**: If the function encounters an unexpected character or reaches the end of the input unexpectedly, it may need to signal an error back to the lexer or parser, depending on the implementation details.

This function plays a crucial role in accurately interpreting numeric literals in the source code, facilitating the conversion of these literals into meaningful tokens for the compiler's subsequent stages.