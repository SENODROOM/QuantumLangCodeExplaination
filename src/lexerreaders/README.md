# LexerReaders Component of the Quantum Language Compiler

## Overview

The `lexerreaders` component in the Quantum Language (QL) compiler is essential for converting source code into a stream of tokens. This step is fundamental for parsing and understanding the structure of the program. The component consists of several functions that handle different aspects of lexical analysis.

### Key Functions

- **readIdentifierOrKeyword**: This function identifies whether a sequence of characters represents an identifier or a keyword in the quantum language. It checks against predefined lists of keywords and returns the appropriate token type.

## File Structure

This directory contains the following files:

- `LexerReaders.cpp`: Contains the implementation of the `readIdentifierOrKeyword` function and other related functions.
- `LexerReaders.h`: Defines the interface for the `lexerreaders` component, including function declarations and necessary data structures.

## Overall Flow

1. **Initialization**: The `lexerreaders` component initializes with a pointer to the current character in the input source code.
2. **Token Reading**: It reads characters from the input one by one, forming sequences based on the rules of the quantum language.
3. **Classification**: Each sequence is classified as either an identifier or a keyword using the `readIdentifierOrKeyword` function.
4. **Output Tokens**: The classified tokens are outputted to the parser for further processing.

## Usage Example

Here's a simple example of how you might use the `lexerreaders` component:

```cpp
#include "LexerReaders.h"

int main() {
    const char* sourceCode = "qubit q0; H q0;";
    TokenStream tokens = readTokens(sourceCode);

    while (!tokens.isEmpty()) {
        Token token = tokens.getNext();
        std::cout << "Token Type: " << token.type << ", Value: " << token.value << std::endl;
    }

    return 0;
}
```

In this example, the `readTokens` function would be part of the `lexerreaders` component, handling the entire lexical analysis process and returning a stream of tokens.

## Contributing

Contributions to the `lexerreaders` component are welcome! If you find any issues or have suggestions for improvements, please open an issue on the [GitHub repository](https://github.com/your-repo/ql-compiler).

---

Feel free to update or expand upon this README as needed to better reflect the specifics of your `lexerreaders` component.