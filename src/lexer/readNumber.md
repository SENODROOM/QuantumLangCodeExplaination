# readNumber Function - Numeric Literal Tokenization

## Overview
The `readNumber()` function is a comprehensive numeric literal parser designed to recognize and tokenize various number formats in source code, including decimal numbers, hexadecimal literals, floating-point numbers, and C-style type suffixes. This function ensures accurate parsing of numeric literals by handling different types of inputs and stripping any unnecessary suffixes.

## Parameters
- **None**: The `readNumber()` function operates directly on the global state of the lexer, which includes the current character (`current`), the position in the source code (`pos`), and the line and column information (`line`, `col`). It does not take any explicit parameters.

## Return Value
- **Token**: The function returns a `Token` object representing the numeric literal. The `TokenType` is set to `NUMBER`, and the `value` field contains the parsed numeric string.

## How It Works
1. **Initialization**:
   - The function starts by recording the starting line and column positions using `startLine` and `startCol`.
   - An empty string `num` is initialized to store the numeric literal being parsed.

2. **Hexadecimal Parsing**:
   - If the current character is `'0'` and the next character is either `'x'` or `'X'`, indicating a hexadecimal literal, the function advances twice to skip over these characters.
   - It then enters a loop that continues as long as there are more characters in the source code and the current character is a valid hexadecimal digit (`std::isxdigit(current())`).
   - Each valid hexadecimal digit is appended to the `num` string.

3. **Decimal and Floating-Point Parsing**:
   - If the current character is not `'0'` followed by `'x'` or `'X'`, the function assumes it's a decimal number or floating-point number.
   - It enters a loop that continues as long as there are more characters in the source code and the current character is either a digit (`std::isdigit(current())`) or a dot (`current() == '.'`).
   - Inside this loop, if a dot is encountered, the function checks if a dot has already been found in the same number. If so, it breaks out of the loop to avoid treating multiple dots as part of a single float.
   - Each valid digit or dot is appended to the `num` string.

4. **Type Suffix Stripping**:
   - After parsing the main numeric part, the function strips any C-style type suffixes (like `LL`, `ULL`, `LU`, `L`, `U`, `F`, `f`) from the end of the `num` string.
   - These suffixes are consumed but not added to the `num` string itself.

5. **Token Creation**:
   - Finally, the function creates and returns a `Token` object with the `TokenType` set to `NUMBER`, the `value` set to the parsed numeric string `num`, and the original line and column positions.

## Edge Cases
- **Empty Input**: If the input source code is empty, the function will not enter any loops and will return an empty `Token` object.
- **Invalid Hexadecimal**: If the sequence after `'0x'` is not a valid hexadecimal digit, the function will stop parsing at the first invalid character.
- **Multiple Dots**: If a numeric literal contains multiple dots, only the first one will be considered part of the floating-point number; subsequent dots will cause the function to exit the loop early.
- **No Type Suffix**: If no type suffix is present after the numeric literal, the function will simply return the parsed numeric string without any modifications.

## Interactions with Other Components
- **Lexer State**: The `readNumber()` function relies on the global state of the lexer, accessing and modifying variables like `current`, `pos`, `line`, and `col`. It updates these variables as it progresses through the source code.
- **Tokenization Process**: This function is called during the tokenization process when the lexer encounters a numeric literal. It plays a crucial role in breaking down the source code into meaningful tokens that can be further processed by the compiler.

By understanding how `readNumber()` functions, developers can better grasp the intricacies of numeric literal parsing in the Quantum Language compiler and ensure that their source code is correctly interpreted and compiled.