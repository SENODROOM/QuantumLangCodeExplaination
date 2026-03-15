# Lexer.cpp - The Quantum Language Compiler's Lexical Analyzer

## Overview

`Lexer.cpp` is a crucial component of the Quantum Language compiler, responsible for converting the input source code into a sequence of tokens. This process is essential for syntax analysis and semantic processing. The lexer reads through the source text character by character, identifying meaningful units such as keywords, identifiers, literals, operators, and punctuation.

## Key Design Decisions

### Tokenization Strategy

The decision to use a `std::unordered_map` for keyword mapping was made to optimize lookup times. Unordered maps provide average-case O(1) time complexity for lookups, which is ideal for quickly identifying reserved words during lexical analysis.

### Error Handling

Incorporating error handling within the lexer allows for early detection and reporting of issues such as unrecognized characters or unterminated strings. This proactive approach ensures that subsequent stages of the compiler can operate on valid input without encountering unexpected errors.

### Character Stream Management

Using a single pass through the source code with functions like `current()`, `peek(int offset)`, and `advance()` simplifies the implementation of the lexer. These functions manage the position in the string efficiently, allowing easy navigation and lookahead capabilities without duplicating code.

## Class and Function Documentation

### Lexer Class

**Purpose:**  
The `Lexer` class encapsulates the logic for converting the source code into tokens. It maintains the state of the lexer, including the current position, line number, and column number.

**Behavior:**  
- **Constructor (`Lexer(const std::string &source)`):** Initializes the lexer with the provided source code.
- **`current()` Method:** Returns the character at the current position in the source code.
- **`peek(int offset)` Method:** Returns the character at the specified offset ahead of the current position.
- **`advance()` Method:** Advances the lexer to the next character and updates the line and column numbers accordingly.
- **`skipWhitespace()` Method:** Skips over any whitespace characters in the source code, updating the position and line/column tracking.
- **Tokenization Methods:** Various methods like `nextToken()`, `number()`, `identifier()`, etc., handle specific types of token extraction.

### Keywords Map

**Purpose:**  
The `keywords` map stores all recognized keywords and their corresponding token types. This map is used to identify reserved words during the tokenization process.

**Behavior:**  
- **Initialization:** The map is initialized with a set of predefined keywords and their associated token types.
- **Lookup:** During tokenization, the lexer checks if the current sequence of characters matches any keyword in the map.

## Tradeoffs and Limitations

### Keyword Case Sensitivity

While the lexer correctly identifies both lowercase and uppercase versions of keywords like `true` and `True`, this could be seen as a limitation. However, it aligns with the language's conventions and provides flexibility in how keywords are written.

### Single Pass Implementation

The single-pass nature of the lexer simplifies the implementation but may limit the ability to perform complex lookahead operations. For languages with more intricate grammar rules, additional passes or more sophisticated parsing techniques might be necessary.

### Error Reporting

The lexer currently reports errors using simple messages. While this is sufficient for basic error handling, more advanced features like error recovery or detailed diagnostic information might be required for larger projects.

## Conclusion

`Lexer.cpp` plays a vital role in the Quantum Language compiler by breaking down the source code into manageable tokens. Its efficient design and robust error handling make it a solid foundation for further development of the compiler. By understanding the key design decisions and limitations, developers can better appreciate the challenges and opportunities in building a comprehensive and reliable compiler.