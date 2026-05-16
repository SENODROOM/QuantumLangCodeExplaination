# `readString` Function

## Overview

The `readString` function is responsible for lexically analyzing and extracting string literals from the source code being processed by the Quantum Language compiler's lexer. This function ensures that string literals are correctly identified and handled, including support for escape sequences within the strings.

### Parameters

- **None**: The function operates directly on the internal state of the lexer, which includes the current position (`pos`), line number (`line`), column number (`col`), and the source code (`src`).

### Return Value

- **Token**: The function returns a `Token` object representing the extracted string literal. The token includes the type (`TokenType::STRING`), the value of the string, and the starting line and column numbers in the source code.

### Edge Cases

1. **Unterminated String Literal**: If the function encounters an opening quote without a corresponding closing quote before reaching the end of the source code, it throws a `QuantumError` with the message "Unterminated string literal" and the starting line number.
2. **Escape Sequences**: The function handles escape sequences such as `\n`, `\t`, `\r`, `\\`, `'`, `"`, and `\0`. Each escape sequence is replaced by its corresponding character in the resulting string.

### Interactions with Other Components

- **Lexer State**: The function modifies the lexer's internal state by advancing the position (`pos`) through the source code. It also updates the line and column numbers accordingly.
- **Tokenization**: The function plays a crucial role in the tokenization process by identifying and extracting string literals. These tokens are then used by subsequent stages of the compiler to parse and generate the appropriate abstract syntax tree (AST).

## Detailed Explanation

The `readString` function begins by recording the starting line and column numbers of the string literal. It then advances the lexer's position past the opening quote. The function enters a loop where it reads characters from the source code until it encounters the closing quote or reaches the end of the source code.

Inside the loop:
- If the current character is an escape sequence (`\`), the function skips the escape character and processes the next character according to the following rules:
  - `\n`: Adds a newline character (`'\n'`) to the string.
  - `\t`: Adds a tab character (`'\t'`) to the string.
  - `\r`: Adds a carriage return character (`'\r'`) to the string.
  - `\\`: Adds a backslash character (`'\\'`) to the string.
  - `'`: Adds a single quote character (`'\''`) to the string.
  - `"`: Adds a double quote character (`'"'`) to the string.
  - `\0`: Adds a null character (`'\0'`) to the string.
  - Any other character after `\`: Adds the character itself to the string.
- If the current character is not an escape sequence, the function simply adds the character to the string and advances the position.

After exiting the loop, the function checks if the string was terminated properly by encountering the closing quote. If not, it throws a `QuantumError` indicating an unterminated string literal. Finally, the function advances the position past the closing quote and returns a `Token` object representing the string literal.

This function is essential for accurately parsing string literals in the Quantum Language source code, ensuring that they are correctly interpreted and represented in the compiler's intermediate representation.