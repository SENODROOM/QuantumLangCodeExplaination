# Lexer Component - Quantum Language Compiler

The Lexer component is a crucial part of the Quantum Language compiler responsible for breaking down the source code into tokens that can be processed by the parser. This README provides an overview of the Lexer's functionality, its components, and the overall flow of tokenization.

## Overview

The Lexer reads through the input source code character by character, identifying patterns that match specific token types such as keywords, identifiers, numbers, strings, and operators. These tokens form the basis of the syntactic analysis performed by the parser.

## Components

### Lexer Class

- **Constructor**: Initializes the Lexer with the input source code.
- **current()**: Returns the current token being processed.
- **peek()**: Returns the next token without advancing the lexer.
- **advance()**: Advances the lexer to the next token.
- **skipWhitespace()**: Skips over any whitespace characters in the input.
- **skipComment()**: Skips over single-line comments starting with `//`.
- **skipBlockComment()**: Skips over multi-line comments enclosed within `/* */`.
- **readNumber()**: Reads a sequence of digits to form a number token.
- **readString()**: Reads a sequence of characters enclosed within quotes to form a string token.

### TokenType Enum

Defines various token types used in the quantum language, including:

- `IDENTIFIER`: Represents variable or function names.
- `NUMBER`: Represents numeric literals.
- `STRING`: Represents string literals.
- `KEYWORD`: Represents reserved words like `let`, `const`, `fn`, etc.
- `OPERATOR`: Represents arithmetic, logical, and other operators.
- `PUNCTUATION`: Represents punctuation marks like commas, semicolons, etc.
- `EOF`: Represents the end of the file.

### Error Handling

The Lexer includes error handling mechanisms to manage unexpected characters or malformed tokens. It throws exceptions when encountering errors, which are then caught and handled by the compiler.

## Overall Flow

1. **Initialization**: The Lexer is initialized with the input source code.
2. **Tokenization Loop**: The lexer enters a loop where it repeatedly calls `advance()` to process each token.
3. **Skip Whitespace and Comments**: During processing, the lexer skips over whitespace and comments using `skipWhitespace()`, `skipComment()`, and `skipBlockComment()`.
4. **Read Tokens**: The lexer identifies and reads different types of tokens such as numbers, strings, and keywords using `readNumber()`, `readString()`, and pattern matching.
5. **Error Handling**: If an unexpected character or malformed token is encountered, the lexer throws an exception, which is then handled by the compiler.
6. **End of File**: When the end of the file is reached, the lexer returns a `TokenType::EOF` token to indicate the completion of tokenization.

## Usage Example

```cpp
#include "Lexer.h"

int main() {
    std::string sourceCode = "let x = 42; // This is a comment";
    Lexer lexer(sourceCode);

    while (lexer.current().type != TokenType::EOF) {
        Token token = lexer.advance();
        std::cout << "Type: " << token.type << ", Value: " << token.value << std::endl;
    }

    return 0;
}
```

In this example, the Lexer processes the input source code and prints out each token type and value.

## Documentation

For detailed documentation on each function and class, refer to the individual header files located in the `../include` directory.

---

This README provides a comprehensive overview of the Lexer component, its functionality, and the overall flow of tokenization in the Quantum Language compiler. For more detailed information, please consult the individual header files and documentation.