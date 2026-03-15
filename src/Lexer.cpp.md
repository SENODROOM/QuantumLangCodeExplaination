# Lexer.cpp - The Quantum Language Compiler's Lexical Analyzer

## Overview

`Lexer.cpp` is a crucial part of the Quantum Language compiler, focusing on the lexical analysis phase where it converts the input source code into a sequence of tokens. These tokens serve as the foundation for subsequent stages of the compiler, including parsing and semantic analysis. The primary responsibilities of `Lexer.cpp` include:

- Identifying and categorizing characters into meaningful tokens.
- Handling whitespace and comments to ensure they do not interfere with tokenization.
- Maintaining accurate line and column numbers for error reporting.

## Role in the Compiler Pipeline

The lexer operates at the beginning of the compilation process, taking raw source code as input and producing a stream of tokens. Each token represents a syntactic unit like keywords, identifiers, literals, operators, and punctuation. This output is then consumed by the parser, which constructs an abstract syntax tree (AST) based on these tokens.

## Key Design Decisions

### Tokenization Rules

Deciding on comprehensive yet flexible tokenization rules was essential. The lexer must accurately distinguish between different types of tokens, such as keywords, identifiers, and literals. For instance, handling both `true` and `True` as valid boolean literals required careful consideration of case sensitivity.

### Error Handling

Implementing robust error handling mechanisms allowed the lexer to gracefully manage unexpected characters or malformed tokens. By throwing exceptions with detailed error messages, the lexer ensures that issues can be traced back to specific lines and columns in the source code.

### Whitespace and Comments

Designing how the lexer should handle whitespace and comments was a significant challenge. Ignoring unnecessary spaces improves readability but complicates tracking positions within the source code. Similarly, treating comments as non-existent during tokenization simplifies the parsing stage but requires additional logic to reconstruct them for error messages.

## Documentation of Major Classes/Functions

### Lexer Class

**Purpose:** Manages the lexical analysis process, converting source code into tokens.

**Behavior:** 
- Initializes with the source code string.
- Iterates through the source code, advancing position and updating line/column numbers.
- Recognizes keywords, identifiers, literals, operators, and punctuation, returning corresponding token types.
- Handles whitespace and comments by skipping them or treating them as non-existent.

### Keywords Map

**Purpose:** Maps string representations of keywords to their respective token types.

**Behavior:** 
- Contains predefined mappings for all supported keywords in the Quantum Language.
- Case-insensitive matching for boolean literals (`true`, `True`) and `nil`.

### current() Function

**Purpose:** Returns the character at the current position in the source code.

**Behavior:** 
- Checks if the current position is within bounds; returns the character at `src[pos]`.
- If out of bounds, returns `\0`.

### peek(int offset) Function

**Purpose:** Allows looking ahead in the source code without advancing the position.

**Behavior:** 
- Returns the character at the specified offset relative to the current position.
- If the offset goes beyond the end of the source code, returns `\0`.

### advance() Function

**Purpose:** Advances the lexer to the next character in the source code.

**Behavior:** 
- Increments the position counter.
- Updates line and column numbers when encountering a newline character.
- Returns the character that was previously at the current position.

### skipWhitespace() Function

**Purpose:** Skips over any whitespace characters in the source code.

**Behavior:** 
- Continuously advances the lexer until a non-whitespace character is encountered.
- Handles newlines appropriately by updating line and column numbers.

## Tradeoffs and Limitations

### Case Sensitivity

While supporting case-insensitive keyword recognition enhances language flexibility, it also increases complexity in the lexer implementation. Ensuring consistent behavior across different cases is crucial for maintaining the integrity of the tokenization process.

### Limited Comment Support

Currently, the lexer supports basic single-line comments starting with `//`. Extending comment support to multi-line comments or block comments would require more sophisticated parsing logic, potentially impacting performance.

### Operator Overloading

The lexer does not currently support operator overloading, which could limit the expressiveness of the language. Implementing operator overloading would necessitate changes to the lexer to recognize and differentiate overloaded operators from standard ones.

## Conclusion

`Lexer.cpp` plays a vital role in the Quantum Language compiler by transforming source code into a sequence of tokens. Its design decisions balance functionality with simplicity, ensuring efficient and accurate lexical analysis. While there are limitations and areas for improvement, the existing implementation forms a solid foundation for further development.