# Lexer.cpp - The Quantum Language Compiler's Lexical Analyzer

## Overview

`Lexer.cpp` is a critical component of the Quantum Language compiler, responsible for converting the input source code into a sequence of tokens. This process is fundamental for syntax analysis and semantic processing. The lexer iterates over the source text character by character, identifying meaningful units such as keywords, identifiers, literals, operators, and punctuation.

## Role in the Compiler Pipeline

The lexer operates at the beginning of the compilation process. It takes the raw source code as input and produces a stream of tokens. These tokens are then consumed by the parser to construct an abstract syntax tree (AST). The lexer ensures that the source code is correctly interpreted and breaks it down into manageable parts for further processing.

## Key Design Decisions

### Tokenization Strategy

The decision to use a state machine for tokenization was made to efficiently handle different types of characters and sequences. A state machine allows the lexer to transition between states based on the current character, enabling it to recognize complex patterns like numbers, strings, and operators.

**Why:** State machines provide a structured and efficient way to process input streams, making them ideal for lexing tasks where multiple rules need to be applied sequentially.

### Handling Unicode Characters

To support a wide range of programming languages and scripts, the lexer includes logic to handle Unicode characters. This ensures that the compiler can accurately parse source code written in various international alphabets.

**Why:** Supporting Unicode characters broadens the scope of the language, allowing developers from around the world to contribute to the project without encountering encoding issues.

### Error Handling

Effective error handling is crucial for any compiler. `Lexer.cpp` implements robust error reporting mechanisms to identify and report lexical errors such as invalid characters, unterminated strings, and unexpected end-of-file conditions. Errors are logged with their line and column positions, facilitating quick debugging.

**Why:** Accurate error reporting helps developers understand and fix issues in their source code more effectively, improving overall development productivity.

## Documentation of Major Classes/Functions

### Lexer Class

**Purpose:** The `Lexer` class encapsulates the functionality required to tokenize the source code. It maintains the current position in the source text, tracks line and column numbers, and provides methods to read and advance through the input.

**Behavior:** The constructor initializes the lexer with the source code string. The `current()` method returns the character at the current position. The `peek(int offset)` method looks ahead by a specified number of characters without advancing the position. The `advance()` method moves to the next character and updates the line and column counters accordingly. The `skipWhitespace()` method skips over whitespace characters until a non-whitespace character is encountered.

### skipWhitespace Function

**Purpose:** The `skipWhitespace()` function advances the lexer past any leading whitespace characters, including spaces, tabs, and newlines.

**Behavior:** It repeatedly calls the `advance()` method until a non-whitespace character is found. Newline characters increment the line counter and reset the column counter.

### Keywords Mapping

**Purpose:** The `keywords` unordered map stores keyword strings and their corresponding `TokenType`. This mapping facilitates quick lookup of keywords during tokenization.

**Behavior:** When a potential keyword is identified, the lexer checks against this map to determine if it is indeed a keyword and assigns the appropriate `TokenType`.

## Tradeoffs or Limitations

- **Performance:** While state machines offer efficiency, they can become complex when handling numerous token types and edge cases.
  
- **Unicode Support:** Although Unicode characters are supported, the lexer may still encounter issues with certain script-specific characters or combining marks.

- **Error Reporting:** The current error reporting mechanism focuses on basic lexical errors. More sophisticated error recovery techniques could improve the user experience but would add complexity to the implementation.

## Conclusion

`Lexer.cpp` plays a vital role in the Quantum Language compiler by breaking down the source code into tokens. Its design decisions, particularly the use of a state machine and comprehensive keyword mapping, ensure that the lexer can handle a variety of inputs accurately and efficiently. However, the lexer also faces challenges related to performance, Unicode support, and error reporting, which require ongoing optimization and enhancement.