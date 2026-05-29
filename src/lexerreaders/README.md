# LexerReaders Component of the Quantum Language Compiler

The `lexerreaders` component is an essential part of the Quantum Language (QL) compiler, tasked with parsing and converting the textual input of the source code into individual tokens. These tokens serve as the fundamental building blocks for subsequent phases of compilation, including syntax analysis and semantic interpretation. The primary goal of this component is to accurately identify and categorize the various elements within the source code, ensuring they are correctly processed in the next stages of the compilation process.

## Overview

The `lexerreaders` component consists of two main functions:

1. **readString**: This function processes string literals in the source code. It identifies the start and end of a string, handles escape sequences, and returns the tokenized string.
2. **readIdentifierOrKeyword**: This function recognizes identifiers and keywords in the source code. It checks if the sequence of characters matches any predefined keywords or if it's a valid identifier name and returns the corresponding token type.

These functions work together to ensure that all parts of the source code are properly tokenized, providing a clear and structured representation of the code that can be easily understood and processed by the rest of the compiler.

## Directory Structure

```
lexerreaders/
├── lexer.cpp
├── lexer.h
└── README.md
```

- **lexer.cpp**: Contains the implementation of the `readString` and `readIdentifierOrKeyword` functions.
- **lexer.h**: Defines the interfaces for the `readString` and `readIdentifierOrKeyword` functions, along with any necessary data structures and constants.
- **README.md**: Provides documentation and overview of the `lexerreaders` component.

## Functionality

### readString

The `readString` function takes a character stream from the source code and processes it to identify string literals. It performs the following steps:

1. Identifies the start of a string literal using quotation marks (`"`).
2. Reads characters until it encounters the matching closing quotation mark.
3. Handles escape sequences within the string, such as `\n`, `\t`, and `\\`.
4. Returns the tokenized string literal.

### readIdentifierOrKeyword

The `readIdentifierOrKeyword` function processes sequences of characters to determine whether they represent an identifier or a keyword. It follows these steps:

1. Checks if the sequence of characters matches any predefined keywords in the Quantum Language.
2. If not, it verifies if the sequence forms a valid identifier name according to the language's rules.
3. Returns the appropriate token type: either `IDENTIFIER` or `KEYWORD`.

## Usage

To use the `lexerreaders` component in your Quantum Language compiler, you need to include the `lexer.h` header file and call the `readString` and `readIdentifierOrKeyword` functions with the appropriate parameters. Here's an example:

```cpp
#include "lexer.h"

int main() {
    std::string sourceCode = "\"Hello, World!\" quantum";
    
    // Tokenize the source code
    Token token1 = readString(sourceCode);
    Token token2 = readIdentifierOrKeyword(sourceCode);
    
    // Output the tokens
    std::cout << "Token 1: " << token1.value << " (" << token1.type << ")" << std::endl;
    std::cout << "Token 2: " << token2.value << " (" << token2.type << ")" << std::endl;
    
    return 0;
}
```

In this example, the `readString` function processes the string literal `"Hello, World!"` and returns a token of type `STRING`. The `readIdentifierOrKeyword` function processes the word `"quantum"` and returns a token of type `KEYWORD`.

## Conclusion

The `lexerreaders` component plays a vital role in the Quantum Language compiler by accurately tokenizing the source code. By understanding and utilizing the functionality provided by this component, developers can build robust and efficient compilers capable of handling complex Quantum Language programs.