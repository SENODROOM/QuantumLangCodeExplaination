# LexerReaders Component of the Quantum Language Compiler

The `lexerreaders` component is a crucial part of the Quantum Language (QL) compiler, responsible for interpreting and converting the raw text of the source code into discrete tokens. These tokens form the basis for further stages of compilation, such as syntax analysis and semantic processing. This component ensures that the source code is accurately parsed and structured before moving on to more complex analyses.

## Overview

The `lexerreaders` component includes two main functions:

1. **readString**: Parses string literals from the source code.
2. **readIdentifierOrKeyword**: Identifies and categorizes identifiers and keywords in the source code.

These functions work together to tokenize the input source code efficiently and accurately.

## Directory Structure

```
lexerreaders/
├── lexer.cpp
├── lexer.h
├── token.cpp
├── token.h
└── README.md
```

- **lexer.cpp** and **lexer.h**: Contains the implementation and declaration of the lexer class, which manages the tokenization process.
- **token.cpp** and **token.h**: Defines the structure and methods for representing tokens within the system.
- **README.md**: This file provides detailed documentation about the `lexerreaders` component.

## How It Works

### Tokenization Process

1. **Initialization**: The lexer reads the source code character by character.
2. **Character Classification**: Each character is classified based on its type (e.g., alphabetic, numeric, special).
3. **Token Formation**: Based on the classification, tokens are formed. For example, sequences of alphabetic characters followed by optional underscores or digits are identified as identifiers or keywords.
4. **String Handling**: String literals are enclosed in quotes and treated as single tokens.
5. **Output**: Tokens are outputted for further processing by the compiler.

### readString Function

The `readString` function handles the parsing of string literals. It looks for characters enclosed in double quotes (`"`). If it encounters a backslash (`\`) followed by another quote, it treats the second quote as part of the string rather than the end of it. This allows for the inclusion of double quotes within string literals without terminating them prematurely.

```cpp
// Example usage of readString
std::string str = readString(sourceCode);
if (!str.empty()) {
    // Process the string literal
}
```

### readIdentifierOrKeyword Function

The `readIdentifierOrKeyword` function identifies and categorizes identifiers and keywords. It checks if the sequence of characters matches any predefined keywords. If not, it categorizes the sequence as an identifier.

```cpp
// Example usage of readIdentifierOrKeyword
Token token = readIdentifierOrKeyword(sourceCode);
if (token.isKeyword()) {
    // Handle keyword
} else {
    // Handle identifier
}
```

## Overall Flow

1. **Source Code Reading**: The lexer starts reading the source code character by character.
2. **Token Classification**: Characters are classified into categories like alphabetic, numeric, or special.
3. **Token Formation**: Depending on the category, tokens are either formed directly or processed by specialized functions like `readString` and `readIdentifierOrKeyword`.
4. **Token Output**: Formed tokens are passed to the next stage of the compiler for further processing.

By following this structured approach, the `lexerreaders` component ensures that the source code is correctly tokenized, facilitating efficient and accurate compilation processes.