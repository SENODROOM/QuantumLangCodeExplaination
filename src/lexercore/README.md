# LexerCore Component of the Quantum Language Compiler

## Overview

The `lexercore` component is a fundamental part of the Quantum Language (QL) compiler responsible for converting source code into a sequence of tokens. This step is critical as it lays the groundwork for the parsing and compilation phases. The main objective of `lexercore` is to precisely identify and classify lexical elements such as keywords, identifiers, literals, and operators.

## Components

### current
- **Description**: Returns the current token being processed.
- **Usage**: Used to access the most recently identified token during the lexing phase.

### peek
- **Description**: Returns the next token without advancing the lexer's position.
- **Usage**: Useful for lookahead scenarios where the next token needs to be examined without consuming it.

### advance
- **Description**: Advances the lexer to the next token.
- **Usage**: Called iteratively to move through the source code sequentially.

### skipWhitespace
- **Description**: Skips over any whitespace characters encountered in the source code.
- **Usage**: Ensures that only meaningful characters are considered during tokenization.

### skipComment
- **Description**: Skips over single-line comments in the source code.
- **Usage**: Removes comment text from the input stream, focusing on executable code.

### skipBlockComment
- **Description**: Skips over multi-line block comments in the source code.
- **Usage**: Handles comments that span multiple lines, ensuring they do not interfere with the tokenization process.

### readNumber
- **Description**: Reads a numeric literal from the source code, handling both integer and floating-point values.
- **Usage**: Converts sequences of digits into appropriate number tokens.

## Flow

1. **Initialization**: The lexer starts by initializing itself with the source code.
2. **Tokenization Loop**:
   - The `advance` function is called repeatedly to iterate through the source code.
   - Inside the loop, the lexer checks for different types of tokens using helper functions like `skipWhitespace`, `skipComment`, `skipBlockComment`, and `readNumber`.
3. **Output**: Each identified token is outputted in sequence, forming the foundation for further parsing and compilation steps.

## Usage Example

```cpp
#include "LexerCore.h"

int main() {
    Lexer lexer("example.q");
    Token token;

    while ((token = lexer.advance()) != Token::END_OF_FILE) {
        std::cout << "Token: " << token.type << ", Value: " << token.value << std::endl;
    }

    return 0;
}
```

In this example, the `Lexer` class uses the `lexercore` component to tokenize a quantum program file named `example.q`. The loop continues until the end of the file is reached, printing out each token type and value.

## Documentation

For detailed documentation on each function and its implementation, please refer to the individual header files within the `lexercore` directory. Each function is thoroughly commented to explain its purpose, parameters, and return values.