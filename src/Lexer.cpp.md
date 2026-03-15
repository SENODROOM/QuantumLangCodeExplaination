# Lexer.cpp - The Quantum Language Compiler's Lexical Analyzer

## Overview

`Lexer.cpp` is a vital part of the Quantum Language compiler, tasked with performing the lexical analysis phase. This phase converts the input source code into a series of tokens, which are then used by the parser to build the abstract syntax tree (AST). The lexer handles various elements such as keywords, operators, identifiers, literals, and whitespace, ensuring accurate parsing.

## Key Design Decisions

### Tokenization Rules

- **Keywords**: A comprehensive list of language-specific keywords is defined using an unordered map (`keywords`). Each keyword maps to a corresponding `TokenType`.
  
  ```cpp
  const std::unordered_map<std::string, TokenType> Lexer::keywords = {
      {"let", TokenType::LET},
      {"const", TokenType::CONST},
      // ... other keywords
  };
  ```

  **Why**: This approach allows for efficient lookup and ensures that all recognized keywords are handled uniformly.

### Character Handling

- **Current Character**: The `current()` function returns the character at the current position.
  
  ```cpp
  char Lexer::current() const
  {
      return pos < src.size() ? src[pos] : '\0';
  }
  ```
  
  **Why**: It simplifies accessing the current character without checking bounds manually.

- **Peek Function**: The `peek(int offset)` function allows looking ahead by a specified number of characters without advancing the position.
  
  ```cpp
  char Lexer::peek(int offset) const
  {
      size_t p = pos + offset;
      return p < src.size() ? src[p] : '\0';
  }
  ```
  
  **Why**: This is useful for lookahead in parsing rules that require context beyond the current token.

- **Advance Function**: The `advance()` function moves to the next character, updating the line and column numbers accordingly.
  
  ```cpp
  char Lexer::advance()
  {
      char c = src[pos++];
      if (c == '\n')
      {
          line++;
          col = 1;
      }
      else
          col++;
      return c;
  }
  ```
  
  **Why**: Keeping track of line and column numbers helps in generating meaningful error messages.

### Whitespace Skipping

- **SkipWhitespace Function**: The `skipWhitespace()` function advances past any whitespace characters, including comments.
  
  ```cpp
  void Lexer::skipWhitespace()
  {
      while (pos < src.size() && (current() == ' ' || current() == '\t' || current() == '\r'))
          advance();
  }
  ```
  
  **Why**: Ignoring whitespace is essential for parsing, but it can clutter the source code. By skipping it automatically, we maintain cleaner source code while still allowing for proper parsing.

## Major Classes/Functions Documentation

### Lexer Class

**Purpose**: Manages the lexical analysis process, converting source code into tokens.

**Behavior**:
- Initializes with the source code string.
- Provides methods to access and advance through the source code.
- Handles tokenization based on predefined rules.

### Keywords Map

**Purpose**: Maps language keywords to their respective token types.

**Behavior**:
- Contains a set of keywords and their associated `TokenType`.
- Used during the tokenization process to identify and categorize keywords.

### current() Function

**Purpose**: Returns the current character being processed.

**Behavior**:
- Checks if the current position is within the bounds of the source code.
- Returns the character at the current position or null terminator if out of bounds.

### peek(int offset) Function

**Purpose**: Allows peeking ahead in the source code by a specified number of characters.

**Behavior**:
- Calculates the position based on the current position and the given offset.
- Returns the character at the calculated position or null terminator if out of bounds.

### advance() Function

**Purpose**: Advances the lexer to the next character and updates line and column tracking.

**Behavior**:
- Moves the current position forward by one character.
- Updates the line and column numbers when encountering newline characters.

### skipWhitespace() Function

**Purpose**: Skips over whitespace characters and comments in the source code.

**Behavior**:
- Continuously advances the lexer until a non-whitespace character is encountered.
- Handles both single-line and multi-line comments.

## Tradeoffs and Limitations

- **Complexity**: Managing a large number of keywords and handling different types of comments adds complexity to the lexer.
  
  **Solution**: Using an unordered map for keywords provides efficient lookup, reducing overall complexity.

- **Error Handling**: The lexer currently lacks detailed error handling for malformed tokens.
  
  **Solution**: Implementing more robust error handling mechanisms will improve the user experience by providing clearer and more actionable error messages.

- **Performance**: Frequent updates to line and column numbers might impact performance, especially for very long source files.
  
  **Solution**: Optimizing the update mechanism could help mitigate potential performance issues.

This README provides a detailed overview of the `Lexer.cpp` file, explaining its role in the compiler pipeline, the reasons behind key design decisions, and documenting each major class and function. It also highlights potential tradeoffs and limitations, guiding future improvements.