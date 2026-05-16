# LexerCore Component of the Quantum Language Compiler

## Overview

The `lexercore` component is a crucial module in the Quantum Language (QL) compiler that converts source code into a sequence of tokens. These tokens serve as the foundation for the parser, which then constructs an abstract syntax tree (AST). The primary goal of `lexercore` is to accurately identify and categorize the lexical elements in the input source code, ensuring that the subsequent parsing process can proceed without errors.

## Key Functionalities

1. **current**: Retrieves the current token from the token stream.
2. **peek**: Looks ahead at the next token in the stream without advancing the position.
3. **advance**: Moves to the next token in the stream.
4. **skipWhitespace**: Skips over any whitespace characters in the source code.
5. **skipComment**: Skips over single-line comments in the source code.
6. **skipBlockComment**: Skips over multi-line block comments in the source code.
7. **readNumber**: Reads a numeric literal from the source code and returns it as a token.

## File Structure

- **LexerCore.h**: Header file containing declarations of all public functions and classes related to the lexer core.
- **LexerCore.cpp**: Source file implementing the functionality declared in `LexerCore.h`.

## Overall Flow

The typical workflow within the `lexercore` component involves the following steps:

1. **Initialization**: The lexer starts by reading the first character from the input source code.
2. **Tokenization**: It processes the input character by character, identifying and classifying them into different token types such as keywords, identifiers, literals, etc.
3. **Skip Whitespace/Comments**: If the lexer encounters whitespace or comments, it skips them appropriately using the `skipWhitespace`, `skipComment`, and `skipBlockComment` functions.
4. **Read Numeric Literals**: For numeric literals, the `readNumber` function is invoked to extract the entire number and return it as a token.
5. **Generate Tokens**: As valid tokens are identified, they are generated and added to the token stream.
6. **Advance Position**: After processing each character, the lexer advances its position in the source code using the `advance` function.
7. **Return Current Token**: Finally, the lexer retrieves and returns the current token using the `current` function.

## Usage Example

Here's a simple example demonstrating how to use the `lexercore` component:

```cpp
#include "LexerCore.h"

int main() {
    std::string sourceCode = "qubit q0; measure q0;";
    LexerCore lexer(sourceCode);

    while (!lexer.isAtEnd()) {
        Token token = lexer.advance();
        std::cout << token.toString() << std::endl;
    }

    return 0;
}
```

In this example, the `LexerCore` object is initialized with a string representing the source code. The lexer then iterates through each token in the source code, printing out its details until the end of the input is reached.

## Conclusion

The `lexercore` component plays a vital role in the Quantum Language compiler by breaking down the source code into manageable tokens. Its robust implementation ensures accurate lexical analysis, enabling the parser to build a reliable AST. By understanding and utilizing the provided functions, developers can effectively integrate and extend the lexer core to support additional features and languages.