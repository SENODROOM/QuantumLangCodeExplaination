# Lexer.cpp - The Quantum Language Compiler's Lexical Analyzer

## Overview

`Lexer.cpp` is a crucial component of the Quantum Language compiler, responsible for the lexical analysis phase. This phase converts the input source code into a sequence of tokens, essential for the parser to accurately interpret and construct the abstract syntax tree (AST). The lexer processes various elements like keywords, operators, identifiers, literals, and whitespace, ensuring that the compiler can proceed to the syntactic and semantic phases without errors.

## Key Design Decisions

### Tokenization Rules

The decision to use a combination of regular expressions and state machines was made to balance flexibility and performance. Regular expressions provide an efficient way to match complex patterns, while state machines allow for more nuanced handling of edge cases and context-dependent lexing rules.

### Error Handling

Implementing robust error handling mechanisms ensures that the lexer can gracefully handle unexpected characters and malformed tokens. By throwing exceptions when encountering errors, the lexer alerts the parser about issues early in the compilation process, facilitating easier debugging and maintenance.

### Whitespace Management

Deciding to treat whitespace as significant but not directly generating tokens allowed for cleaner parsing logic. This approach simplifies the parser by removing the need to explicitly handle spaces between tokens, focusing instead on meaningful token sequences.

## Documentation

### Lexer Class

**Purpose:** Manages the lexical analysis of the source code, converting it into a stream of tokens.

**Behavior:** 
- Initializes with the source code string.
- Advances through the source code, identifying and categorizing tokens.
- Handles errors and skips over whitespace.

### Keywords Map

**Purpose:** Maps common keywords in the Quantum Language to their corresponding `TokenType`.

**Behavior:** 
- Contains predefined keyword strings and their associated types.
- Used during tokenization to quickly identify and classify keywords.

### Current Function

**Purpose:** Returns the character at the current position in the source code.

**Behavior:** 
- Checks if the current position is within bounds.
- Returns the character at the current position; otherwise, returns `\0`.

### Peek Function

**Purpose:** Allows peeking ahead in the source code by a specified offset.

**Behavior:** 
- Calculates the position based on the current position and the provided offset.
- Returns the character at the calculated position; otherwise, returns `\0`.

### Advance Function

**Purpose:** Advances the lexer to the next character in the source code.

**Behavior:** 
- Increments the position pointer.
- Updates line and column numbers accordingly.
- Returns the advanced character.

### SkipWhitespace Function

**Purpose:** Skips over any whitespace characters in the source code.

**Behavior:** 
- Continuously advances the lexer until a non-whitespace character is encountered.
- Handles newline characters by updating line and column numbers.

## Tradeoffs and Limitations

- **Complexity:** The combined use of regular expressions and state machines adds complexity to the lexer implementation.
- **Performance:** While regular expressions offer high performance for pattern matching, managing states efficiently requires careful optimization.
- **Edge Cases:** Handling all possible edge cases and context-dependent lexing rules can be challenging and may require additional testing.
- **Language Variants:** Supporting multiple language variants or extensions might necessitate adjustments to the lexer, potentially increasing its complexity.

This README provides a comprehensive overview of the `Lexer.cpp` file, detailing its functionality, design decisions, and potential tradeoffs. For further details, refer to the source code and accompanying documentation.