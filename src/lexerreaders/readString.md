# readString Function

## Overview

The `readString` function is responsible for parsing and extracting string literals from the source code during the lexical analysis phase of the Quantum Language compiler. This function ensures that string literals are accurately recognized and handled, including the processing of escape sequences within the strings.

### Why It Works This Way

The `readString` function operates in a loop to consume characters from the input source until it encounters the closing quotation mark. During each iteration, it checks if the current character is an escape sequence (`\`). If it is, the function processes the escape sequence according to its type (e.g., `\n`, `\t`, etc.) and appends the corresponding character or sequence to the string being built. If the escape sequence is not recognized, the function simply appends the character as is. This approach allows the function to correctly interpret and handle string literals containing special characters.

### Parameters/Return Value

- **Parameters**:
  - None

- **Return Value**:
  - A `Token` object representing the parsed string literal. The token includes the following information:
    - `TokenType::STRING`: Indicates that the token represents a string literal.
    - `str`: The content of the string literal.
    - `startLine`: The starting line number where the string literal begins.
    - `startCol`: The starting column number where the string literal begins.

### Edge Cases

1. **Unterminated String Literal**: If the function reaches the end of the source code without encountering the closing quotation mark, it throws a `QuantumError` indicating an unterminated string literal. This error helps catch syntax errors early in the compilation process.

2. **Empty String Literal**: An empty string literal (`""`) would be processed normally, returning a `Token` with an empty string content.

3. **Escape Sequence Errors**: If an unrecognized escape sequence is encountered (e.g., `\x`), the function appends the sequence as is. However, this could lead to unexpected behavior at runtime. To handle such cases, additional validation might be required after the lexer stage.

4. **Multi-line Strings**: Although not explicitly supported in this implementation, handling multi-line strings would involve reading characters until a newline character followed by the closing quotation mark is encountered.

### Interactions With Other Components

The `readString` function interacts with other components of the Quantum Language compiler through the following mechanisms:

- **Lexer Class**: This function is part of the `Lexer` class, which handles the overall lexical analysis of the source code. The `Lexer` class uses `readString` to parse string literals and generate corresponding tokens.

- **Source Code Buffer**: The function reads characters from the source code buffer stored in the `src` member variable of the `Lexer` class. It advances the position pointer (`pos`) and updates the line and column counters (`line` and `col`) as it consumes characters.

- **Token Generation**: After successfully parsing a string literal, the function generates a `Token` object and returns it. This token can then be used by subsequent stages of the compiler, such as the parser, to construct the abstract syntax tree (AST).

Overall, the `readString` function plays a crucial role in ensuring that string literals are correctly identified and processed during the lexical analysis phase of the Quantum Language compiler. Its implementation leverages simple character-by-character parsing techniques combined with conditional logic to handle escape sequences effectively.