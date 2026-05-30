# LexerCore Component of the Quantum Language Compiler

## Overview

The `lexercore` component is a crucial module in the Quantum Language (QL) compiler that converts source code into a sequence of tokens. This process is essential for subsequent stages such as parsing and compiling. The primary goal of `lexercore` is to accurately identify and categorize lexical elements from the input source code.

### Key Features

- **Tokenization**: Converts raw text into meaningful tokens.
- **Error Handling**: Detects and reports syntax errors during tokenization.
- **Whitespace and Comment Skipping**: Ignores unnecessary whitespace and comments to focus on significant lexical elements.

## File Structure

This directory contains several key files and functions that work together to achieve the tokenization process:

- **current**: Returns the current character being processed without advancing the position.
- **peek**: Returns the next character without advancing the position.
- **advance**: Advances the position to the next character.
- **skipWhitespace**: Skips over any whitespace characters in the input.
- **skipComment**: Skips over single-line comments starting with `//`.
- **skipBlockComment**: Skips over multi-line comments enclosed within `/* */`.
- **readNumber**: Reads a numeric literal from the input and returns it as a token.

## Overall Flow

1. **Initialization**: The lexer starts at the beginning of the input source code.
2. **Character Processing**: It processes one character at a time using functions like `current`, `peek`, and `advance`.
3. **Whitespace and Comment Handling**: Any leading whitespace or comments are skipped using `skipWhitespace`, `skipComment`, and `skipBlockComment`.
4. **Token Identification**: Based on the characters processed, the lexer identifies different types of tokens such as keywords, identifiers, literals, etc.
5. **Error Detection**: If an invalid character or syntax error is encountered, appropriate error messages are generated.
6. **Output**: Tokens are collected and passed to the parser for further processing.

## Usage Example

Here's a simple example demonstrating how you might use the `lexercore` component:

```cpp
#include "lexercore.h"

int main() {
    std::string sourceCode = "qubit q0; // Initialize quantum register\n";
    Lexer lexer(sourceCode);

    while (!lexer.isAtEnd()) {
        Token token = lexer.advance();
        std::cout << token.type << ": " << token.lexeme << "\n";
    }

    return 0;
}
```

In this example, the `Lexer` class uses the functions defined in `lexercore` to tokenize the provided source code and print out each token type and lexeme.

## Contributing

Contributions to the `lexercore` component are welcome! Please ensure your changes are well-documented and tested before submitting a pull request.

---

This README provides a clear overview of the `lexercore` component, its functionality, and how it fits into the broader context of the Quantum Language compiler.