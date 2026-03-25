# LexerReaders Component of the Quantum Language Compiler

The `lexerreaders` component is a crucial part of the Quantum Language (QL) compiler, responsible for reading and parsing tokens from the source code into meaningful components that can be further processed by the compiler's parser. This component includes two primary functions: `readString` and `readIdentifierOrKeyword`.

## Overview

The `lexerreaders` component operates at the lexical analysis stage of the compilation process. Its main goal is to convert raw text input into a sequence of tokens, which represent the smallest units of meaning in the language. These tokens are then used by the parser to construct the abstract syntax tree (AST).

## Files and Functions

### readString

- **Purpose**: Reads a string literal from the source code.
- **Parameters**:
  - `input`: A reference to the input stream containing the source code.
  - `startPos`: The starting position of the token in the source code.
- **Return Value**: A `Token` object representing the string literal.
- **Description**: This function handles the reading of string literals, including escape sequences and ensuring that the string is properly terminated. It returns a `Token` object with the type set to `STRING` and the value set to the parsed string.

### readIdentifierOrKeyword

- **Purpose**: Reads an identifier or keyword from the source code.
- **Parameters**:
  - `input`: A reference to the input stream containing the source code.
  - `startPos`: The starting position of the token in the source code.
- **Return Value**: A `Token` object representing the identifier or keyword.
- **Description**: This function distinguishes between identifiers and keywords. Identifiers are names used to identify variables, functions, or other entities, while keywords have special meanings within the language. The function returns a `Token` object with the appropriate type (`IDENTIFIER` or `KEYWORD`) and value based on the content of the source code.

## Overall Flow

1. **Initialization**: The lexer starts by initializing its state, including setting up the input stream and preparing any necessary data structures for tokenization.
2. **Character Reading**: Characters are read from the input stream one by one.
3. **Token Recognition**: Depending on the character, the lexer recognizes different types of tokens such as strings, identifiers, keywords, operators, etc.
4. **Token Construction**: For recognized tokens, the lexer constructs `Token` objects using the `readString` and `readIdentifierOrKeyword` functions.
5. **Error Handling**: If the lexer encounters invalid characters or unterminated strings, it raises errors accordingly.
6. **Output**: Tokens are outputted sequentially, forming the basis for subsequent parsing stages.

## Usage Example

```cpp
#include "LexerReaders.h"

int main() {
    std::istringstream input("Hello, world! let x = 10;");
    Token token;
    
    // Read a string
    token = readString(input, Position(1, 1));
    std::cout << "Token Type: " << token.type << ", Token Value: " << token.value << std::endl;

    // Read an identifier or keyword
    token = readIdentifierOrKeyword(input, Position(1, 14));
    std::cout << "Token Type: " << token.type << ", Token Value: " << token.value << std::endl;

    return 0;
}
```

In this example, the `lexerreaders` component reads a string and an identifier/keyword from a sample input stream and prints their respective types and values.

## Conclusion

The `lexerreaders` component plays a vital role in the Quantum Language compiler by converting raw text into structured tokens. Understanding and utilizing these functions effectively is essential for developers working on the lexer and parser phases of the compiler.