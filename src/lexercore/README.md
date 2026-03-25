# LexerCore Component of the Quantum Language Compiler

The `lexercore` component is a fundamental part of the Quantum Language (QL) compiler, responsible for converting source code into a stream of tokens that can be processed by the parser. This README provides an overview of the key functionalities and their relationships within the component.

## Key Functions/Files

1. **current**: Returns the current character being processed without advancing the position in the input stream.
2. **peek**: Returns the next character in the input stream without advancing the position.
3. **advance**: Advances the position in the input stream by one character and returns the character that was just read.
4. **skipWhitespace**: Skips over any whitespace characters (spaces, tabs, newlines) until it encounters a non-whitespace character or reaches the end of the input.
5. **skipComment**: Skips over single-line comments starting with `//` until the end of the line.
6. **skipBlockComment**: Skips over multi-line block comments enclosed between `/*` and `*/`.
7. **readNumber**: Reads a sequence of digits from the input stream, forming a numeric token.

## Overall Flow

The primary flow through the `lexercore` component involves:

1. **Initialization**: The lexer starts at the beginning of the input source code.
2. **Tokenization**:
   - It uses the `current`, `peek`, and `advance` functions to navigate through the input.
   - Whitespace and comments are skipped using `skipWhitespace`, `skipComment`, and `skipBlockComment`.
   - Numeric tokens are identified and formed using `readNumber`.
   - Other types of tokens (e.g., keywords, identifiers, operators) are recognized based on specific patterns and rules.
3. **Output**: Tokens are generated and returned as they are identified, forming the intermediate representation used by the parser.

## File Relationships

- **LexerCore.h**: Contains declarations for all the public functions and classes related to the lexer core.
- **LexerCore.cpp**: Implements the functionality declared in `LexerCore.h`. Each function is designed to handle a specific aspect of tokenization.
- **Tokenizer.h**: Defines the interface for the tokenizer, which includes methods like `nextToken()` that utilize the core functions.
- **Tokenizer.cpp**: Provides implementations for the tokenizer interface, integrating the core functions to produce a continuous stream of tokens.

## Usage Example

Here's a simple example demonstrating how you might use the `lexercore` component to tokenize a piece of quantum source code:

```cpp
#include "LexerCore.h"
#include "Tokenizer.h"

int main() {
    std::string sourceCode = "qubit q0; // Declare a qubit\n";
    Tokenizer tokenizer(sourceCode);
    
    while (true) {
        Token token = tokenizer.nextToken();
        if (token.type == TokenType::EOF) break;
        std::cout << token.toString() << std::endl;
    }
    
    return 0;
}
```

In this example, the `Tokenizer` class uses the `lexercore` functions to process the `sourceCode` string and output each token individually.

## Conclusion

The `lexercore` component plays a crucial role in the Quantum Language compiler by breaking down the source code into manageable tokens. Its modular design allows for easy integration and extension, making it a vital foundation for the entire compilation process.