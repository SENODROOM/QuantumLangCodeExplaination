# LexerReaders Component of the Quantum Language Compiler

## Overview

The `lexerreaders` component in the Quantum Language (QL) compiler is responsible for lexical analysis, which involves breaking down the source code into meaningful units called tokens. This process is essential for the subsequent stages of compilation, including syntax analysis and semantic interpretation. The component provides two primary functions:

1. **readString**: Parses strings enclosed in double quotes (`"`) or single quotes (`'`) from the input source code.
2. **readIdentifierOrKeyword**: Identifies and categorizes identifiers and keywords based on their names and context.

These functions work together to ensure that the source code is correctly tokenized, allowing the compiler to proceed with further processing.

## File Structure

- **lexerreaders.h**: Header file containing declarations of public functions and data structures used by the lexerreaders component.
- **lexerreaders.cpp**: Source file implementing the logic for reading strings and identifying identifiers or keywords.

## Overall Flow

1. **Initialization**: The lexerreaders component initializes itself with the input source code.
2. **Tokenization**:
   - **readString**: Scans the input source code for string literals, using either double or single quotes as delimiters. It handles escape sequences within the strings.
   - **readIdentifierOrKeyword**: Identifies words in the input source code as either identifiers (variable names, function names, etc.) or keywords (reserved words like `qubit`, `measure`, etc.). It uses a predefined list of keywords and checks the context to differentiate between them.
3. **Output**: The component outputs the tokens generated during the lexical analysis phase. These tokens are then consumed by the parser for further syntactic and semantic processing.

## Usage Example

```cpp
#include "lexerreaders.h"

int main() {
    std::string sourceCode = "qubit q; measure q;";
    LexerReaders lexer(sourceCode);
    Token token;

    while (lexer.getNextToken(token)) {
        // Process each token
        std::cout << "Token type: " << token.type << ", Value: " << token.value << std::endl;
    }

    return 0;
}
```

In this example, the `LexerReaders` class is initialized with a sample quantum language source code. The `getNextToken` method is repeatedly called to retrieve tokens until there are no more tokens left. Each token's type and value are printed to demonstrate the output of the lexerreaders component.

## Documentation

For detailed documentation on the functions and classes within the `lexerreaders` component, please refer to the [Doxygen-generated documentation](https://www.doxygen.nl/).

By following the steps outlined in the documentation, developers can understand how to use and integrate the `lexerreaders` component into their Quantum Language compilers.