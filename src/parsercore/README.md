# ParserCore Component of the Quantum Language Compiler

The `ParserCore` component is a crucial module within the Quantum Language (QL) compiler, responsible for transforming the input source code into an Abstract Syntax Tree (AST). This process validates the source code against the language's syntactical rules, facilitating precise execution during subsequent compilation phases.

## Overview

- **check**: Validates the current token against expected tokens and throws an error if they do not match.
- **match**: Consumes the current token if it matches the expected token type; otherwise, throws an error.
- **atEnd**: Checks if the end of the input has been reached.
- **skipNewlines**: Skips over any newline characters in the input.
- **parse**: Initiates the parsing process, constructing the AST from the input source code.

## Directory Structure

```
parsercore/
├── check.cpp
├── check.h
├── match.cpp
├── match.h
├── atEnd.cpp
├── atEnd.h
├── skipNewlines.cpp
├── skipNewlines.h
└── parse.cpp
    └── parse.h
```

### Files

1. **check.cpp/check.h**:
   - Contains functions to validate the current token against expected tokens.
   - Used to ensure the correct sequence of tokens before proceeding with parsing.

2. **match.cpp/match.h**:
   - Implements logic to consume the current token if it matches the expected token type.
   - Essential for advancing through the token stream during parsing.

3. **atEnd.cpp/atEnd.h**:
   - Provides functionality to determine if the end of the input has been reached.
   - Used to control the flow of the parsing process when there are no more tokens to process.

4. **skipNewlines.cpp/skipNewlines.h**:
   - Includes methods to skip over any newline characters in the input.
   - Ensures that the parser ignores unnecessary whitespace, focusing only on meaningful syntax elements.

5. **parse.cpp/parse.h**:
   - Orchestrates the entire parsing process, utilizing the aforementioned helper functions.
   - Constructs the AST by recursively breaking down the source code into its constituent parts.

## Overall Flow

1. **Initialization**: The parser initializes itself with the input source code and prepares to start parsing.
2. **Tokenization**: The input source code is tokenized into individual lexical units.
3. **Parsing**:
   - The `parse` function begins the parsing process.
   - It uses `check`, `match`, `atEnd`, and `skipNewlines` functions to validate, consume, and manage the token stream respectively.
   - The parser constructs the AST based on the grammar rules defined for the Quantum Language.
4. **Error Handling**: If the parser encounters unexpected tokens or reaches the end of the input unexpectedly, it throws appropriate errors.
5. **Completion**: Once the parsing is complete, the AST is ready for further processing by the compiler.

By ensuring accurate syntax validation and efficient token management, the `ParserCore` component plays a pivotal role in the successful compilation of Quantum Language programs.