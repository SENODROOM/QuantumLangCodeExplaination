# Lexer.cpp - The Quantum Language Compiler's Lexical Analyzer

## Overview

`Lexer.cpp` is a critical component of the Quantum Language compiler, specifically designed for the lexical analysis phase. This phase converts the input source code into a sequence of tokens, which serve as the fundamental building blocks for further processing by the parser. The lexer handles a variety of lexical elements such as keywords, operators, identifiers, literals, and whitespace, ensuring accurate parsing of the source code.

## Key Design Decisions

### Tokenization Strategy
The decision was made to use a combination of character-by-character scanning and lookahead to accurately identify and categorize tokens. This approach allows for the detection of multi-character tokens like `==`, `!=`, and `&&`.

**Why:** Multi-character tokens often require more than one character to be recognized correctly. A single-pass character-by-character scan would miss these cases, leading to incorrect tokenization.

### Handling Whitespace
A dedicated function `skipWhitespace()` is implemented to handle all forms of whitespace, including spaces, tabs, and newlines. This ensures that the lexer can ignore unnecessary characters without affecting the token stream.

**Why:** Ignoring whitespace is essential for maintaining readability and simplicity in the source code. However, it must be done carefully to avoid misinterpreting the structure of the code.

### Error Handling
The lexer includes robust error handling mechanisms to manage unexpected characters and malformed tokens. Errors are reported using custom exceptions defined in the `Error.h` header file.

**Why:** Accurate error reporting is crucial for debugging and improving the quality of the compiler. It helps developers understand where issues lie in their code and how to fix them.

## Documentation of Major Classes/Functions

### Lexer Class
- **Purpose:** Manages the lexical analysis process.
- **Behavior:** Initializes with the source code string and iteratively processes it to generate tokens.

### skipWhitespace Function
- **Purpose:** Skips over any whitespace characters in the source code.
- **Behavior:** Advances the position pointer until a non-whitespace character is encountered.

### advance Function
- **Purpose:** Moves to the next character in the source code.
- **Behavior:** Returns the current character and advances the position pointer. Handles newline characters by incrementing the line number and resetting the column number.

### current Function
- **Purpose:** Retrieves the current character being processed.
- **Behavior:** Returns the character at the current position pointer. If the end of the source code is reached, returns `\0`.

### peek Function
- **Purpose:** Allows looking ahead at a specified number of characters in the source code.
- **Behavior:** Returns the character at the current position plus the specified offset. If the end of the source code is reached, returns `\0`.

## Tradeoffs and Limitations

### Complexity
The implementation of multi-character token detection adds complexity to the lexer. While this enhances accuracy, it also increases the overhead during tokenization.

**Limitation:** The lexer may become slower on larger source files due to the increased number of checks required for multi-character tokens.

### Case Sensitivity
While the lexer recognizes some keywords case-insensitively (e.g., `true` and `True`), others remain sensitive. This could lead to confusion when writing code across different platforms or languages.

**Limitation:** Developers should be aware of the case sensitivity rules for keywords and ensure consistency in their code.

### Unicode Support
The current implementation does not support Unicode characters beyond basic ASCII. Extending the lexer to handle Unicode would require additional logic and testing.

**Limitation:** The lexer may not correctly interpret or tokenize source code containing non-ASCII characters.

## Conclusion

`Lexer.cpp` plays a vital role in the Quantum Language compiler by converting source code into a sequence of tokens. Its design choices focus on accuracy, simplicity, and robust error handling. While there are some tradeoffs and limitations, the lexer provides a solid foundation for the subsequent phases of the compilation process.