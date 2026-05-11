# LexerReaders Component of the Quantum Language Compiler

The `lexerreaders` component is a crucial part of the Quantum Language (QL) compiler, responsible for reading and parsing tokens from the source code into meaningful elements that can be further processed by the compiler's parser. This component primarily consists of two functions: `readString` and `readIdentifierOrKeyword`.

## Overview

The `lexerreaders` component plays a pivotal role in tokenizing the input source code. It reads characters from the input stream and converts them into tokens that represent different types of elements such as strings, identifiers, keywords, and operators. The tokens are then passed on to the parser for syntactic analysis.

### Functions

1. **`readString`**:
   - **Purpose**: Reads a string literal from the source code.
   - **Process**: 
     1. Starts reading from the current position until it encounters a closing quote (`"`).
     2. Handles escape sequences within the string.
     3. Returns the parsed string token.
   
2. **`readIdentifierOrKeyword`**:
   - **Purpose**: Reads an identifier or keyword from the source code.
   - **Process**: 
     1. Starts reading from the current position until it encounters a non-alphanumeric character or whitespace.
     2. Checks if the read sequence matches any known keywords.
     3. If it matches a keyword, returns the corresponding keyword token; otherwise, returns an identifier token.

### File Structure

- **`readString.cpp`**: Contains the implementation of the `readString` function.
- **`readIdentifierOrKeyword.cpp`**: Contains the implementation of the `readIdentifierOrKeyword` function.
- **`token.h`**: Defines the structure and types of tokens used by the lexer.

## Usage

To use the `lexerreaders` component, you need to include the necessary header file and call the appropriate functions with the source code as input. Here’s a basic example:

```cpp
#include "token.h"

int main() {
    std::string sourceCode = "\"Hello, World!\" quantum";
    
    // Read a string token
    Token* stringToken = readString(sourceCode);
    std::cout << "String Token: " << stringToken->value << std::endl;
    
    // Read an identifier or keyword token
    Token* idOrKeywordToken = readIdentifierOrKeyword(sourceCode);
    std::cout << "ID/Keyword Token: " << idOrKeywordToken->value << std::endl;
    
    return 0;
}
```

## Contributing

Contributions to the `lexerreaders` component are welcome. Please follow these steps to contribute:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Make your changes and commit them.
4. Push your changes to your forked repository.
5. Open a pull request to merge your changes into the main branch.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

Feel free to update the content based on additional details or requirements specific to your project.