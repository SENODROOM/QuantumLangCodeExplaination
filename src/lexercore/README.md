# LexerCore Component of the Quantum Language Compiler

The `lexercore` component is a crucial part of the Quantum Language (QL) compiler, tasked with transforming source code into a sequence of tokens. These tokens serve as the foundation for subsequent parsing stages in the compilation process. This README offers an overview of the primary functionalities and their interconnections within the component.

## Overview

The `lexercore` component processes the input source code character by character, identifying meaningful units such as keywords, identifiers, literals, and operators. It then converts these units into token objects, which are stored in a buffer for further processing by the parser.

## Key Functions/Files

### 1. current()
- **Description**: Returns the current token being processed without advancing the lexer.
- **Usage**: Useful for checking the next token without consuming it from the stream.

### 2. peek()
- **Description**: Returns the next token in the stream without advancing the lexer.
- **Usage**: Allows lookahead to determine the type or value of the upcoming token.

### 3. advance()
- **Description**: Advances the lexer to the next token in the stream.
- **Usage**: Moves through the source code sequentially, one token at a time.

### 4. skipWhitespace()
- **Description**: Skips over any whitespace characters in the source code.
- **Usage**: Ensures that only significant characters are considered during tokenization.

### 5. skipComment()
- **Description**: Skips over single-line comments in the source code.
- **Usage**: Removes comment text from the source code, preventing it from affecting the token stream.

### 6. skipBlockComment()
- **Description**: Skips over multi-line block comments in the source code.
- **Usage**: Handles comments that span multiple lines, ensuring they do not interfere with tokenization.

### 7. readNumber()
- **Description**: Reads a numeric literal from the source code.
- **Usage**: Identifies and extracts numbers, converting them into appropriate token types.

## Overall Flow

1. **Initialization**: The lexer starts with an empty token buffer and sets up the initial state.
2. **Character Processing**: The lexer reads characters from the input source code one by one.
3. **Token Identification**: Each character is analyzed to determine if it belongs to a token unit (e.g., keyword, identifier, number).
4. **Token Creation**: When a complete token unit is identified, a corresponding token object is created and added to the buffer.
5. **Skimming**: Whitespace and comments are skipped to ensure that only relevant tokens are processed.
6. **Buffer Management**: Tokens are managed in a buffer, allowing for easy access and manipulation by the parser.
7. **Completion**: The lexer continues until all characters in the source code have been processed, resulting in a fully populated token buffer.

By following this structured approach, the `lexercore` component efficiently transforms the raw source code into a tokenized format, enabling precise parsing and semantic analysis in the Quantum Language compiler.