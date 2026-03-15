# Lexer.h in QuantumLanguage Compiler

## Overview

The `Lexer.h` header file is an integral part of the QuantumLanguage compiler's architecture, responsible for converting raw text into a sequence of tokens. Tokens represent the smallest meaningful units in the language, forming the basis for further parsing and semantic analysis. This file plays a crucial role in the initial phase of the compiler pipeline, where it takes the input source code and breaks it down into manageable pieces.

## Key Design Decisions

### Tokenization Strategy

- **Flexibility**: The lexer supports both regular and template literals, allowing for complex string representations.
- **Efficiency**: Utilizing character lookahead and state transitions to efficiently parse through the source code.
- **Error Handling**: Designed to handle syntax errors gracefully, providing clear error messages indicating the location and nature of issues.

### State Management

- **Current Position Tracking**: Maintaining the position (`pos`) within the source code helps in accurately reporting errors and managing tokenization states.
- **Line and Column Counting**: Keeping track of the current line (`line`) and column (`col`) positions aids in generating precise error messages and debugging information.

## Class and Function Documentation

### Lexer Class

**Purpose**: The primary class responsible for tokenizing the source code.

**Behavior**:
- **Constructor**: Initializes the lexer with the provided source code.
- **tokenize() Method**: Processes the entire source code and returns a vector of tokens.

#### Private Members:

- **src**: Stores the source code being processed.
- **pos**: Tracks the current position within the source code.
- **line**, **col**: Track the current line and column numbers during processing.
- **keywords**: A static unordered map defining reserved keywords and their corresponding token types.
- **pendingTokens_**: Used temporarily during f-string expansion to store intermediate tokens.
- **defines_**: Stores C preprocessor macros, mapping macro names to lists of replacement tokens.

#### Private Methods:

- **current()**: Returns the character at the current position.
- **peek(int offset)**: Returns the character at the specified offset ahead of the current position.
- **advance()**: Advances the current position and returns the character that was previously at that position.
- **skipWhitespace()**: Skips over any whitespace characters until a non-whitespace character is encountered.
- **skipComment()**: Skips over single-line comments starting with `//`.
- **skipBlockComment()**: Skips over multi-line comments enclosed between `/*` and `*/`.
- **readNumber()**: Reads and returns a numeric token (integer or double).
- **readString(char quote)**: Reads and returns a string token using the specified quotation marks.
- **readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)**: Handles template literal parsing, storing intermediate results in `out`.
- **readIdentifierOrKeyword()**: Reads and returns an identifier or keyword token.
- **readOperator()**: Reads and returns an operator token.

## Tradeoffs and Limitations

- **Complexity**: Supporting both regular and template literals adds complexity to the lexer implementation.
- **Performance**: While efficient, the lexer may still be a bottleneck for very large source files due to its need to process every character.
- **Error Reporting**: Precise error messages require accurate tracking of line and column positions, which can add overhead.

This README provides a comprehensive overview of the `Lexer.h` file, detailing its functionality, design decisions, and potential challenges.