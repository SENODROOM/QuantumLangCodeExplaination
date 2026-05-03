# QuantumLanguage Compiler - Lexer.h

## Overview

The `include/Lexer.h` header file is an essential part of the QuantumLanguage compiler, focusing on the lexical analysis phase. The lexer converts source code into a sequence of tokens that can be processed by subsequent phases of the compiler. This file defines the `Lexer` class, which is crucial for parsing the input source code accurately.

## Role in Compiler Pipeline

The lexer operates at the beginning of the compiler pipeline, taking raw source code as input and producing a stream of tokens. These tokens represent the smallest meaningful units of the language, such as keywords, identifiers, literals, operators, and punctuation. The lexer's primary responsibility is to break down the source code into these tokens while preserving their positions and types.

### Key Design Decisions and Why

1. **State Machine Approach**: The lexer uses a state machine to efficiently parse the source code. This approach allows for clear separation of concerns and easier maintenance of the parser logic.
2. **Tokenization Rules**: Detailed rules are defined for recognizing different types of tokens, including numbers, strings, identifiers, and operators. This ensures that the lexer can handle various syntactic elements of the language correctly.
3. **Error Handling**: Robust error handling mechanisms are implemented to manage syntax errors gracefully. The lexer provides information about the location and nature of errors, facilitating debugging during the development process.

## Major Classes/Functions Overview

### Lexer Class

- **Constructor (`explicit Lexer(const std::string &source)`)**: Initializes the lexer with the source code string.
- **tokenize Method (`std::vector<Token> tokenize()`)**: Main method that processes the source code and returns a vector of tokens.

### Private Methods

- **current Method (`char current() const`)**: Returns the character at the current position in the source code.
- **peek Method (`char peek(int offset = 1) const`)**: Returns the character at the specified offset without advancing the position.
- **advance Method (`char advance()`)**: Advances the position in the source code and returns the character at the new position.
- **skipWhitespace Method (`void skipWhitespace()`)**: Skips over any whitespace characters in the source code.
- **skipComment Method (`void skipComment()`)**: Skips over single-line comments starting with `//`.
- **skipBlockComment Method (`void skipBlockComment()`)**: Skips over multi-line comments enclosed within `/* */`.
- **readNumber Method (`Token readNumber()`)**: Reads and constructs a number token from the source code.
- **readString Method (`Token readString(char quote)`)**: Reads and constructs a string token from the source code using the specified quotation mark.
- **readTemplateLiteral Method (`void readTemplateLiteral(std::vector<Token> &out, int startLine, int startCol)`)**: Handles template literals, allowing for f-string expansion.
- **readIdentifierOrKeyword Method (`Token readIdentifierOrKeyword()`)**: Reads and constructs an identifier or keyword token from the source code.
- **readOperator Method (`Token readOperator()`)**: Reads and constructs an operator token from the source code.

## Tradeoffs

1. **Complexity vs. Performance**: While a more complex lexer might provide better error handling and support for advanced features, it could also impact performance. The current implementation strikes a balance between simplicity and functionality.
2. **Flexibility vs. Consistency**: Allowing for flexible tokenization rules enables the lexer to handle a wide variety of languages and dialects. However, this flexibility can sometimes lead to inconsistencies in how different tokens are parsed.
3. **Memory Usage vs. Parsing Speed**: Storing intermediate results like pending tokens for f-string expansion requires additional memory. Balancing this against parsing speed is crucial for efficient operation.

Overall, the `Lexer.h` file plays a foundational role in the QuantumLanguage compiler by breaking down the source code into meaningful tokens. Its design decisions aim to strike a balance between complexity, flexibility, and performance, ensuring robust and efficient lexical analysis.