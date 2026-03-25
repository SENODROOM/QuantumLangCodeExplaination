# Token Component Documentation

## Overview

The Token component is a crucial module in the Quantum Language Compiler responsible for parsing and managing tokens extracted from the source code. Tokens represent the basic building blocks of the language, including keywords, identifiers, literals, and operators. This component ensures that the source code is accurately converted into a structured format, enabling further processing by other components of the compiler.

### Key Features

- **Tokenization**: Converts raw text from the source code into individual tokens.
- **Token Representation**: Provides a consistent representation of tokens as strings.
- **Error Handling**: Detects and reports syntax errors during tokenization.

## Files and Functions

### `token.h`

This header file defines the `Token` class, which represents a single token in the source code. It includes methods for creating tokens, accessing their type and value, and converting them to a readable string.

#### Class Definition

```cpp
class Token {
public:
    enum Type { KEYWORD, IDENTIFIER, LITERAL, OPERATOR };

    Token(Type type, std::string value);
    Type getType() const;
    std::string getValue() const;
    std::string toString() const;

private:
    Type type_;
    std::string value_;
};
```

#### Methods

- `Token(Type type, std::string value)`: Constructor to initialize the token's type and value.
- `Type getType() const`: Returns the type of the token.
- `std::string getValue() const`: Returns the value of the token.
- `std::string toString() const`: Converts the token to a human-readable string.

### `tokenizer.cpp`

This source file implements the logic for tokenizing the source code. It reads through the input text and identifies tokens based on predefined rules.

#### Function Definitions

```cpp
#include "token.h"
#include <vector>
#include <string>

std::vector<Token> tokenize(const std::string& sourceCode) {
    // Implementation details
}
```

#### Helper Functions

- `isKeyword(const std::string& str)`: Checks if a given string is a keyword.
- `isIdentifier(const std::string& str)`: Checks if a given string is an identifier.
- `isLiteral(const std::string& str)`: Checks if a given string is a literal.
- `isOperator(char ch)`: Checks if a given character is an operator.

### `tokenizer.h`

This header file declares the `tokenize` function, which takes a string representing the source code and returns a vector of `Token` objects.

#### Function Declaration

```cpp
#include <vector>
#include <string>

std::vector<Token> tokenize(const std::string& sourceCode);
```

## Overall Flow

1. **Source Code Input**: The `tokenize` function receives a string containing the source code.
2. **Character Analysis**: It iterates through each character of the source code.
3. **Token Identification**: Based on character patterns and predefined rules, it identifies and categorizes characters into tokens.
4. **Token Creation**: For each identified token, a `Token` object is created with its appropriate type and value.
5. **Output Vector**: The function collects all created `Token` objects into a vector and returns it.

This modular approach allows for easy maintenance and expansion of the tokenization process, ensuring that the Quantum Language Compiler can handle various types of tokens efficiently.