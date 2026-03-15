# Lexer.cpp - The Quantum Language Compiler's Lexical Analyzer

## Overview

`Lexer.cpp` is a crucial component of the Quantum Language compiler, responsible for the lexical analysis phase. During this phase, the input source code is converted into a sequence of tokens, forming the foundation for subsequent parsing by the compiler. The lexer processes various lexical elements including keywords, operators, identifiers, literals, and whitespace, ensuring accurate parsing of the source code.

## Key Design Decisions

### Tokenization Rules

- **Keywords vs Identifiers**: To distinguish between keywords and identifiers, a lookup table (`keywords`) was used to map strings to their corresponding `TokenType`. This approach ensures that any string matching a keyword is correctly identified.
  
- **Case Insensitivity for Boolean Literals**: Boolean literals like "true" and "True" are considered equivalent. Therefore, both cases are mapped to `TokenType::BOOL_TRUE`, simplifying the handling of boolean values in the source code.

- **Alias Handling**: Certain keywords have aliases in other languages (e.g., "this" and "self" in Python). These aliases are mapped to the same token type, maintaining consistency across different language constructs.

## Class/Function Documentation

### Lexer Class

**Purpose**: The primary class for the lexer, encapsulating all necessary functionality for tokenizing the source code.

**Behavior**:
- **Constructor**: Initializes the lexer with the source code and sets up initial positions and line/column counters.
- **Tokenize Method**: Processes the source code character-by-character, skipping whitespace, identifying keywords, operators, and literals, and returning a vector of tokens.

### Keywords Map

**Purpose**: A constant unordered map that associates strings representing keywords with their respective `TokenType`.

**Behavior**: Used during tokenization to quickly identify and categorize keywords.

### Helper Functions

#### current()

**Purpose**: Returns the current character being processed.

**Behavior**: Checks if the position is within bounds before accessing the source string.

#### peek(int offset)

**Purpose**: Returns the character at a specified offset ahead of the current position without advancing the position.

**Behavior**: Provides lookahead capability for more complex token recognition.

#### advance()

**Purpose**: Advances to the next character in the source code and updates the line and column counters accordingly.

**Behavior**: Handles newline characters to maintain accurate line and column information.

#### skipWhitespace()

**Purpose**: Skips over any whitespace characters in the source code.

**Behavior**: Continuously advances through whitespace until a non-whitespace character is encountered.

## Tradeoffs and Limitations

- **Complexity of Keyword Mapping**: Using a map for keyword mapping adds overhead but allows for efficient lookup and easy maintenance of keyword lists.
  
- **Limited Lookahead**: The current implementation supports basic lookahead using the `peek()` function. Advanced lookahead might be required for certain grammatical structures, potentially increasing complexity.

- **Handling of Aliases**: While aliases simplify syntax consistency, they add additional complexity to the lexer logic. Careful management of these mappings is essential to avoid conflicts.

- **Error Reporting**: Error handling is minimal in this snippet. Proper error reporting should include line numbers and context to aid debugging.

## Usage Example

Below is an example demonstrating how to use the `Lexer` class:

```cpp
#include "Lexer.h"

int main() {
    std::string sourceCode = "let x = 5; print(x);";
    Lexer lexer(sourceCode);
    auto tokens = lexer.tokenize();

    for (const auto &token : tokens) {
        std::cout << "Type: " << static_cast<int>(token.type) 
                  << ", Value: " << token.value 
                  << ", Line: " << token.line 
                  << ", Column: " << token.column << std::endl;
    }

    return 0;
}
```

This example initializes a `Lexer` with a simple source code string and then tokenizes it, printing out each token's details.

By understanding the roles and behaviors of `Lexer.cpp` and its components, developers can effectively utilize this module in the Quantum Language compiler pipeline.