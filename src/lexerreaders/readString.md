# readString Function

## Overview

The `readString` function is responsible for parsing and extracting string literals from the source code during the lexical analysis phase of the Quantum Language compiler. This function ensures that string literals are accurately recognized and handled, including the processing of escape sequences within the strings.

### Why It Works This Way

The function operates by advancing through the source code to identify and extract string literals. It specifically handles escape sequences to allow for special characters such as newlines (`\n`), tabs (`\t`), carriage returns (`\r`), backslashes (`\\`), single quotes (`\'`), double quotes (`\"`), and null characters (`\0`) to be included in the string without being misinterpreted as part of the syntax.

### Parameters/Return Value

- **Parameters**:
  - None
  
- **Return Value**:
  - A `Token` object representing the parsed string literal. The token includes the type (`TokenType::STRING`), the value of the string, and the starting line and column positions in the source code.

### Edge Cases

- **Unterminated String Literal**: If the function encounters an opening quote but not a corresponding closing quote before reaching the end of the source code, it throws a `QuantumError` indicating a lexing error.
  
- **Invalid Escape Sequence**: If the function encounters an invalid escape sequence (i.e., a backslash followed by a character that is not one of the specified escape characters), it simply appends the character to the string.

### Interactions With Other Components

- **Lexer Class**: The `readString` function is called by the `Lexer` class when it detects a string literal in the source code. The lexer uses this function to parse the string and create a `Token` object which is then added to the list of tokens for further processing.
  
- **Source Code Position Tracking**: The function keeps track of the starting position of the string literal using `startLine` and `startCol`. These values are used to set the appropriate location information in the returned `Token` object, ensuring that any errors or issues related to the string can be traced back to its exact position in the source code.