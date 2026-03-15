# readString Function - String Literal Tokenization

## Overview
The `readString()` function is responsible for processing string literals enclosed in either single or double quotes within the source code. This function handles various aspects such as escape sequences, detection of unterminated string literals, and accurate tracking of the token's position in the source code. By comprehensively managing these details, `readString` ensures that string literals are correctly identified and parsed during the lexical analysis phase of the Quantum Language compiler.

## Parameters and Return Value
- **Parameters**:
  - None
- **Return Value**:
  - A `Token` object representing the string literal. The token includes the type (`TokenType::STRING`), the value of the string, and its starting line and column positions in the source code.

## How It Works
1. **Initialization**: 
   - The function starts by recording the current line and column numbers where the string literal begins. These values are stored in `startLine` and `startCol`.
   
2. **Skip Opening Quote**:
   - The first character to be processed is the opening quote, which is skipped using the `advance()` method.

3. **Reading Characters Inside the String**:
   - The function enters a loop that continues until it encounters the closing quote or reaches the end of the source code.
   - If the current character is an escape sequence (`\`), the function processes the next character based on the escape sequence rules:
     - `\n`: Adds a newline character (`'\n'`) to the string.
     - `\t`: Adds a tab character (`'\t'`) to the string.
     - `\r`: Adds a carriage return character (`'\r'`) to the string.
     - `\\`: Adds a backslash character (`'\\'`) to the string.
     - `\'`: Adds a single quote character (`'\''`) to the string.
     - `\"`: Adds a double quote character (`'"'`) to the string.
     - `\0`: Adds a null character (`'\0'`) to the string.
     - Any other character following the backslash is added verbatim to the string.
   - If the current character is not an escape sequence, it is directly added to the string using the `advance()` method.

4. **Handling Unterminated Strings**:
   - If the loop completes without encountering the closing quote, indicating that the string literal is unterminated, the function throws a `QuantumError` with the message "Unterminated string literal" and the recorded starting line number.

5. **Skip Closing Quote**:
   - After successfully reading the entire string literal, including any escape sequences, the function skips the closing quote using the `advance()` method.

6. **Create and Return Token**:
   - Finally, the function creates a `Token` object with the type set to `TokenType::STRING`, the value set to the constructed string, and the starting line and column positions. This token is then returned.

## Edge Cases
- **Empty String Literals**: While not explicitly handled in the provided code snippet, the function should ideally support empty string literals (`""`). However, since the loop condition checks against the end of the source code before checking for the closing quote, an empty string would result in an unterminated string error.
- **Invalid Escape Sequences**: If an invalid escape sequence is encountered (e.g., `\x`), the function should add the escape sequence characters verbatim to the string. This behavior can be adjusted to handle such cases differently, depending on the requirements.
- **Mixed Quotes**: The function currently only supports matching quotes (i.e., if a string is opened with a single quote, it must be closed with a single quote). Supporting mixed quotes (e.g., opening with a single quote and closing with a double quote) would require additional logic.

## Interactions With Other Components
- **Lexer Class**: The `readString()` function is part of the `Lexer` class, which is responsible for breaking down the source code into tokens. Other functions in the `Lexer` class may call `readString()` when they encounter a string literal in the source code.
- **Tokenizer Module**: The `readString()` function interacts with the tokenizer module, which is responsible for parsing the source code and generating tokens. When the tokenizer encounters a string literal, it calls `readString()` to process the literal and create a corresponding token.
- **Error Handling**: The function uses the `throw QuantumError` statement to handle errors related to unterminated string literals. This error handling mechanism is crucial for maintaining the integrity and correctness of the compiled program.