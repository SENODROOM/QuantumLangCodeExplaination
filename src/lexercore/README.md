# LexerCore Component of the Quantum Language Compiler

## Overview

The `lexercore` component is a vital module within the Quantum Language (QL) compiler responsible for converting source code into a sequence of tokens. These tokens form the basis for subsequent parsing steps, ultimately leading to the construction of an Abstract Syntax Tree (AST). The core functionality of `lexercore` includes identifying and categorizing various lexical elements such as keywords, identifiers, literals, and operators.

## Files and Their Roles

### current
- **Role**: Returns the current token without advancing the lexer.
- **Usage**: Useful for lookahead operations where the next token needs to be examined without changing the state of the lexer.

### peek
- **Role**: Similar to `current`, but allows specifying a number of tokens to look ahead.
- **Usage**: Facilitates more complex lookahead scenarios required by certain grammar rules.

### advance
- **Role**: Advances the lexer to the next token.
- **Usage**: Used iteratively to traverse through the entire source code, generating tokens one by one.

### skipWhitespace
- **Role**: Skips over any whitespace characters encountered during tokenization.
- **Usage**: Ensures that tokens are not affected by spaces, tabs, or newlines in the source code.

### skipComment
- **Role**: Skips over single-line comments starting with `//`.
- **Usage**: Removes comment lines from the source code, focusing only on executable code.

### skipBlockComment
- **Role**: Skips over multi-line comments enclosed between `/*` and `*/`.
- **Usage**: Handles block comments that span multiple lines, ensuring they do not interfere with tokenization.

### readNumber
- **Role**: Reads a numeric literal from the source code.
- **Usage**: Converts sequences of digits into appropriate numerical tokens (e.g., integers, floating-point numbers).

## Overall Flow

1. **Initialization**: The lexer starts at the beginning of the source code.
2. **Tokenization**:
   - **Skip Whitespace**: Any leading whitespace is skipped.
   - **Identify Tokens**: The lexer identifies the next token based on its type (keyword, identifier, literal, operator).
   - **Read Number**: If the token is a number, it reads the full numeric literal.
   - **Handle Comments**: Single and multi-line comments are skipped to maintain focus on executable code.
3. **Output**: Each identified token is emitted in sequence.
4. **Completion**: The process continues until the end of the source code is reached.

## Usage Example

```cpp
#include "lexercore.h"

int main() {
    std::string sourceCode = "let x = 5; // This is a comment\n";
    Lexer lexer(sourceCode);

    while (!lexer.isAtEnd()) {
        Token token = lexer.advance();
        std::cout << token.type << ": " << token.lexeme << "\n";
    }

    return 0;
}
```

In this example, the `Lexer` class uses the `lexercore` component to tokenize the provided source code. Each token's type and lexeme are printed out, demonstrating how the lexer processes the input and generates meaningful tokens.

## Conclusion

The `lexercore` component plays a pivotal role in the Quantum Language compiler by breaking down the source code into discrete tokens. Its ability to handle whitespace, comments, and numeric literals ensures that the subsequent parsing stages receive a clean, structured input. By understanding and utilizing the functionalities provided by `lexercore`, developers can effectively build robust and accurate quantum language compilers.