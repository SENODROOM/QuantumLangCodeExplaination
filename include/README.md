# QuantumLanguage Compiler - Lexer.h

## Overview

The `include/Lexer.h` header file is an essential component of the QuantumLanguage compiler's lexical analysis phase. It defines the `Lexer` class responsible for converting the input source code into a sequence of tokens, which are then processed by the parser. The lexer handles the conversion of characters into meaningful units such as identifiers, numbers, strings, operators, and keywords, facilitating the subsequent stages of compilation.

## Key Design Decisions

- **Separation of Concerns**: The lexer is designed to handle the low-level parsing of characters into tokens without worrying about higher-level semantic analysis. This separation ensures that changes in the syntax do not affect other parts of the compiler.
  
- **Efficient Tokenization**: To optimize performance, the lexer uses efficient data structures like vectors and unordered maps. For example, it utilizes an unordered map to quickly identify keywords, reducing the time complexity of token recognition.

- **Support for F-Strings**: The lexer includes support for f-string expansion using a pending tokens buffer. This feature allows for dynamic string generation based on variable values, enhancing the flexibility and usability of the language.

## Class Documentation

### Lexer

**Purpose**: The `Lexer` class is responsible for reading the source code and converting it into a stream of tokens.

**Behavior**:
- **Constructor (`explicit Lexer(const std::string &source)`)**: Initializes the lexer with the source code.
- **Method (`std::vector<Token> tokenize()`)**: Reads through the source code and returns a vector of tokens representing the parsed content.

**Tradeoffs**:
- **Complexity**: Supporting f-string expansion adds complexity to the lexer, requiring additional state management and logic.

### Keywords Map

**Purpose**: A static unordered map that stores keyword strings and their corresponding token types.

**Behavior**:
- Maps specific keyword strings to their respective `TokenType`, aiding in quick identification during tokenization.

## Function Documentation

### current()

**Purpose**: Returns the character at the current position in the source code.

**Behavior**:
- Provides access to the character pointed to by `pos`.

### peek(int offset = 1)

**Purpose**: Returns the character at the specified offset relative to the current position.

**Behavior**:
- Allows lookahead without advancing the position pointer, useful for parsing complex constructs.

### advance()

**Purpose**: Advances the position pointer to the next character in the source code.

**Behavior**:
- Increments `pos` and returns the character that was previously at `pos`.

### skipWhitespace()

**Purpose**: Skips over any whitespace characters in the source code.

**Behavior**:
- Iterates through the source code until a non-whitespace character is encountered, updating `line` and `col` accordingly.

### skipComment()

**Purpose**: Skips over a single-line comment starting with `//`.

**Behavior**:
- Continues reading characters until the end of the line is reached, updating `line` but not `col`.

### skipBlockComment()

**Purpose**: Skips over a multi-line comment enclosed between `/*` and `*/`.

**Behavior**:
- Continues reading characters until both `*/` are found, updating `line` and `col`.

### readNumber()

**Purpose**: Parses a numeric literal from the source code.

**Behavior**:
- Recognizes integer and floating-point literals, returning a `Token` of type `NUMBER`.

### readString(char quote)

**Purpose**: Parses a string literal from the source code.

**Behavior**:
- Handles both single and double-quoted strings, returning a `Token` of type `STRING`.

### readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)

**Purpose**: Parses a template literal (f-string) from the source code.

**Behavior**:
- Expands the f-string by evaluating embedded expressions and concatenating them with string literals, storing the result in `out`.

### readIdentifierOrKeyword()

**Purpose**: Parses an identifier or keyword from the source code.

**Behavior**:
- Determines if the parsed text is an identifier or a keyword, returning a `Token` of type `IDENTIFIER` or `KEYWORD`.

### readOperator()

**Purpose**: Parses an operator from the source code.

**Behavior**:
- Identifies and returns a `Token` representing one of the supported operators.

## Limitations

- **F-String Expansion**: While powerful, the implementation of f-string expansion can lead to increased memory usage and slower parsing times due to the need to evaluate and store intermediate results.
  
- **Error Handling**: Basic error handling mechanisms are implemented, but more sophisticated error reporting could be added for better debugging capabilities.

This README provides a comprehensive overview of the `Lexer.h` file, detailing its functionality, design decisions, and potential limitations.