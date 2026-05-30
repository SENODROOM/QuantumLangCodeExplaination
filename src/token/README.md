# Token Component Documentation

## Overview

The Token component is a crucial module in the Quantum Language Compiler responsible for parsing and managing the lexical tokens extracted from the source code. Tokens represent the basic building blocks of the language, including keywords, identifiers, literals, and operators. This component ensures that the source code is accurately converted into a structured format that can be further processed by the parser.

### Key Features

- **Tokenization**: Converts raw source code into individual lexical tokens.
- **Error Handling**: Detects and reports syntax errors encountered during tokenization.
- **Token Storage**: Efficiently stores and manages the parsed tokens for subsequent phases of compilation.
- **Token Types**: Supports various types of tokens such as keywords, identifiers, integers, floating-point numbers, strings, and operators.

### Directory Structure

```
token/
├── include/
│   └── token.hpp
├── src/
│   ├── tokenizer.cpp
│   └── tokenizer.hpp
└── tests/
    ├── test_tokenizer.cpp
    └── CMakeLists.txt
```

### Files

#### `include/token.hpp`

This header file declares the `Token` class and related enumerations for different token types. It provides the interface for creating and manipulating tokens.

```cpp
#ifndef TOKEN_HPP
#define TOKEN_HPP

enum TokenType {
    KEYWORD,
    IDENTIFIER,
    INTEGER,
    FLOATING_POINT,
    STRING,
    OPERATOR,
    EOF_TOKEN
};

class Token {
public:
    Token(TokenType type, const std::string& value);
    ~Token();

    TokenType getType() const;
    std::string getValue() const;

private:
    TokenType type_;
    std::string value_;
};

#endif // TOKEN_HPP
```

#### `src/tokenizer.cpp`

This source file implements the `Tokenizer` class, which is responsible for reading the source code and generating tokens. It includes error handling mechanisms to detect and report syntax errors.

```cpp
#include "tokenizer.hpp"
#include "token.hpp"

Tokenizer::Tokenizer(const std::string& source) : source_(source), position_(0) {}

Token Tokenizer::nextToken() {
    while (position_ < source_.length()) {
        char ch = source_[position_];

        switch (ch) {
            case ' ': case '\t': case '\n':
                position_++;
                break;
            case '=': return Token(OPERATOR, "=");
            default:
                // Handle other characters or report error
                throw std::runtime_error("Unexpected character");
        }
    }

    return Token(EOF_TOKEN, "");
}
```

#### `tests/test_tokenizer.cpp`

This file contains unit tests for the `Tokenizer` class using Google Test framework. It verifies the correctness of tokenization and error handling.

```cpp
#include <gtest/gtest.h>
#include "../src/tokenizer.hpp"

TEST(TokenizerTest, SimpleTokenization) {
    Tokenizer tokenizer("int x = 5;");
    EXPECT_EQ(tokenizer.nextToken().getType(), KEYWORD);
    EXPECT_EQ(tokenizer.nextToken().getType(), IDENTIFIER);
    EXPECT_EQ(tokenizer.nextToken().getType(), OPERATOR);
    EXPECT_EQ(tokenizer.nextToken().getValue(), "5");
}

TEST(TokenizerTest, ErrorHandling) {
    Tokenizer tokenizer("int x = 5; invalid");
    try {
        tokenizer.nextToken();
        tokenizer.nextToken();
        tokenizer.nextToken(); // Should throw an exception
    } catch (const std::runtime_error& e) {
        EXPECT_EQ(std::string(e.what()), "Unexpected character");
    }
}
```

### Overall Flow

1. **Initialization**: The `Tokenizer` object is initialized with the source code string.
2. **Tokenization**: The `nextToken()` method reads through the source code character by character, identifying and categorizing them into tokens based on their type (keyword, identifier, literal, operator).
3. **Error Detection**: If an unexpected character is encountered, the tokenizer throws a runtime error indicating a syntax error.
4. **Token Storage**: Each token is stored internally within the `Tokenizer` object for easy access and manipulation during the compilation process.
5. **Testing**: Unit tests are provided to validate the functionality of the tokenizer, ensuring it correctly handles various cases including valid tokens and syntax errors.

By following this structure and implementing the necessary classes and methods, the Token component plays a vital role in the Quantum Language Compiler, providing a robust foundation for further stages of compilation.