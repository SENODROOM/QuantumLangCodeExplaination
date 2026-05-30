# LexerReaders Component of the Quantum Language Compiler

## Overview

The `lexerreaders` component in the Quantum Language (QL) compiler is responsible for lexical analysis, which involves breaking down the source code into meaningful units called tokens. This process is crucial for the subsequent stages of compilation, such as syntax analysis and semantic interpretation. The component includes several functions that handle different types of tokenization tasks.

## Files and Functions

### readString

- **Description**: This function reads string literals from the source code. It handles escape sequences within strings and ensures that the string is properly terminated.
- **Parameters**:
  - `input`: A reference to the input stream containing the source code.
  - `token`: A reference to the token where the parsed string will be stored.
- **Returns**: `true` if the string was successfully read; otherwise, `false`.

### readIdentifierOrKeyword

- **Description**: This function identifies whether the current sequence of characters forms an identifier or a keyword in the Quantum Language.
- **Parameters**:
  - `input`: A reference to the input stream containing the source code.
  - `token`: A reference to the token where the parsed identifier or keyword will be stored.
- **Returns**: `true` if the identifier or keyword was successfully read; otherwise, `false`.

## Overall Flow

1. **Initialization**: The lexer starts by reading characters from the source code one by one.
2. **Token Recognition**:
   - For string literals, it calls `readString`.
   - For identifiers or keywords, it calls `readIdentifierOrKeyword`.
3. **Error Handling**: If any invalid character or sequence is encountered during tokenization, appropriate error messages are generated.
4. **Output**: Each recognized token is added to the output list, ready for further processing in the parser.

## Usage Example

```cpp
#include "lexerreaders.h"
#include <iostream>

int main() {
    std::istringstream input("let qubit = 0");
    Token token;
    
    if (readIdentifierOrKeyword(input, token)) {
        std::cout << "Parsed Keyword: " << token.value << std::endl;
    } else if (readString(input, token)) {
        std::cout << "Parsed String: " << token.value << std::endl;
    }
    
    return 0;
}
```

In this example, the `lexerreaders` component would recognize "let" as a keyword and output accordingly.

## Contributing

Contributions to the `lexerreaders` component are welcome! Please ensure your changes are well-documented and tested before submitting a pull request.

## License

The `lexerreaders` component is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.