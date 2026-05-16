# LexerReaders Component of the Quantum Language Compiler

The `lexerreaders` component is an essential part of the Quantum Language (QL) compiler, tasked with interpreting and converting the raw text of the source code into discrete tokens. These tokens serve as the foundational building blocks for subsequent stages of compilation, including syntax analysis and semantic processing. The primary functions within this component are `readString` and `readIdentifierOrKeyword`, which handle specific types of tokenization.

## Overview

The `lexerreaders` component operates on the input source code, breaking it down into individual tokens. Each token represents a syntactic unit such as keywords, identifiers, strings, or operators. The component ensures that the tokens are correctly identified and categorized according to their type, facilitating efficient parsing and error detection in the quantum language.

### Key Functions

1. **readString**: This function is designed to parse string literals from the source code. It identifies the start and end of the string using quotation marks (`"`) and handles escape sequences within the string.

2. **readIdentifierOrKeyword**: This function processes both identifiers and keywords. Identifiers are names used to identify variables, functions, or other entities, while keywords have special meanings in the quantum language itself. The function differentiates between these based on predefined rules and returns the appropriate token type.

### File Structure

- **readString.cpp**: Contains the implementation of the `readString` function, which focuses on parsing string literals.
- **readIdentifierOrKeyword.cpp**: Includes the logic for identifying and categorizing identifiers and keywords.
- **token.h**: Defines the structure and enumeration of tokens used throughout the lexer and parser components.

## Flow Diagram

```plaintext
+---------------------+
|  Source Code        |
+--------+----------+
         |
         v
+--------v----------+
| readString()      |
+--------+----------+
         |
         v
+--------v----------+
| readIdentifierOrKeyword() |
+--------+----------+
         |
         v
+--------v----------+
| Tokenized Output    |
+---------------------+
```

In summary, the `lexerreaders` component plays a pivotal role in the Quantum Language compiler by transforming the source code into a stream of tokens. This process is critical for accurately interpreting the language's syntax and semantics, ensuring that the compiler can proceed with further stages of compilation without errors.