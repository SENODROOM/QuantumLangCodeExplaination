# LexerCore Component of the Quantum Language Compiler

## Overview

The `lexercore` component is a crucial module in the Quantum Language (QL) compiler, tasked with transforming source code into a sequential stream of tokens. This process is essential for the subsequent stages of parsing and compiling the quantum programs. The primary goal of `lexercore` is to accurately identify and categorize the different elements in the source code, such as keywords, identifiers, literals, operators, and punctuation marks, thereby facilitating the creation of an Abstract Syntax Tree (AST).

## Files and Functions

### readNumber

- **Purpose**: This function reads a numeric literal from the input source code. It handles both integer and floating-point numbers, ensuring that they are correctly parsed and categorized as tokens.
- **Parameters**:
  - `input`: A reference to the input stream containing the source code.
- **Return Value**: Returns a token representing the numeric literal read from the input.

## Overall Flow

1. **Initialization**: The lexer starts by initializing its state and preparing to read the source code.
2. **Character Reading**: It reads characters one by one from the input source code.
3. **Token Identification**: Based on the current character(s), it identifies whether the sequence forms a keyword, identifier, literal, operator, or punctuation mark.
4. **Token Creation**: For recognized sequences, it creates corresponding token objects.
5. **Error Handling**: If an unrecognized sequence is encountered, it raises an error indicating invalid syntax.
6. **Output Stream**: Tokens are collected and outputted through a stream, ready for further processing by the parser.

## Dependencies

- `token.h`: Header file defining the structure of a token.
- `errorhandler.h`: Header file for handling errors during lexical analysis.

## Usage Example

```cpp
#include "lexercore.h"
#include "token.h"

int main() {
    std::istringstream input("qubit q = 0;");
    TokenStream tokens = lexercore::readTokens(input);

    while (!tokens.empty()) {
        Token token = tokens.front();
        tokens.pop_front();
        // Process each token
    }

    return 0;
}
```

In this example, the `lexercore::readTokens` function is used to convert the input string into a stream of tokens. Each token can then be processed according to the needs of the compiler's parsing stage.

## Conclusion

The `lexercore` component plays a foundational role in the Quantum Language compiler by breaking down source code into meaningful tokens. Its efficient and accurate implementation ensures that the subsequent stages of compilation proceed smoothly, ultimately resulting in the generation of executable quantum programs.