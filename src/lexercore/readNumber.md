# `readNumber` Function

## Purpose
The `readNumber` function is designed to parse numeric literals from the source code input of a quantum language compiler. It identifies both integers and floating-point numbers, including those in hexadecimal format. This function ensures that the numeric values are correctly extracted and classified into appropriate token types based on their format.

## Parameters
- None explicitly declared as parameters; instead, it uses member variables like `line`, `col`, `pos`, and `src`.

## Return Value
- Returns a `Token` object representing the numeric literal parsed. The token type is set to `TokenType::NUMBER`, and the token's lexeme (the actual number) is stored in the `num` string.

## How It Works
1. **Initialization**: 
   - The function initializes `startLine` and `startCol` with the current line and column positions (`line` and `col`). These represent the starting position of the numeric literal being read.
   - An empty string `num` is initialized to store the parsed numeric literal.
   - A boolean flag `hasDot` is set to `false` to track whether a decimal point has been encountered during parsing.

2. **Hexadecimal Parsing**:
   - If the current character is `'0'` and the next character is either `'x'` or `'X'`, indicating a hexadecimal number, the function advances the position twice to skip over the initial `'0x'`.
   - It then enters a loop where it continues to advance the position until it encounters a non-hexadecimal digit. All characters within this loop are added to the `num` string, forming the hexadecimal number.

3. **Decimal Parsing**:
   - If the current character is not `'0'` followed by `'x'` or `'X'`, the function assumes a decimal number.
   - It enters a loop where it continues to advance the position as long as the current character is a digit or a decimal point.
     - If a decimal point is encountered, the function checks if a decimal point has already been found (`hasDot`). If so, it breaks out of the loop to avoid parsing invalid floating-point numbers.
     - Otherwise, it sets `hasDot` to `true` and adds the decimal point to the `num` string.
   - After exiting the loop, the function strips any C-style integer or float suffixes (like `'L'`, `'l'`, `'U'`, `'u'`, `'F'`, `'f'`) from the end of the `num` string. These suffixes are consumed but not added to the final numeric literal.

4. **Creating and Returning Token**:
   - Finally, the function creates a `Token` object with the type `TokenType::NUMBER`, the lexeme `num`, and the starting line and column positions.
   - This token is returned to be used in further processing by the compiler.

## Edge Cases
- **Empty Input**: If there are no characters left to process (`pos >= src.size()`), the function will return an empty `Token` object.
- **Invalid Hexadecimal Format**: If the sequence starts with `'0x'` but contains non-hexadecimal digits, the function will stop at the first invalid digit.
- **Multiple Decimal Points**: If multiple decimal points are encountered (e.g., `123.456.`), the function will stop at the second decimal point, treating the rest as part of the comment or invalid syntax.
- **Suffix Stripping**: Any trailing suffixes like `'L'`, `'l'`, `'U'`, `'u'`, `'F'`, `'f'` are removed, ensuring that only the numeric value is captured.

## Interactions with Other Components
- **Lexer Core**: This function is called by the lexer core when it encounters a potential numeric literal. The lexer core manages the overall state and flow of the lexical analysis process.
- **Tokenization**: The resulting `Token` object is used in the tokenization phase, which breaks down the source code into individual tokens. These tokens are then passed to the parser for further syntactic analysis.
- **Error Handling**: While not shown in the provided code snippet, this function likely interacts with error handling mechanisms to report invalid numeric formats or missing suffixes.

By understanding how `readNumber` parses different numeric formats and handles various edge cases, developers can better integrate this function into the broader context of a quantum language compiler.