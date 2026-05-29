# ParserCore Component of the Quantum Language Compiler

## Overview

The `ParserCore` component is a critical module within the Quantum Language (QL) compiler, responsible for converting input source code into an Abstract Syntax Tree (AST). This process ensures that the source code strictly conforms to the language’s syntactical rules, facilitating subsequent compilation steps and enhancing error detection during the parsing phase.

### Key Features

- **Syntax Checking**: Validates the input source code against the defined syntax rules.
- **Token Matching**: Identifies and matches tokens based on their type and value.
- **Error Handling**: Detects and reports syntax errors in the source code.
- **AST Generation**: Constructs an AST from the validated and matched tokens, representing the structure of the quantum program.

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

### Files and Their Functions

#### `check.cpp` / `check.h`

These files contain functions related to syntax checking. They validate individual tokens and ensure they adhere to the syntactical rules of the Quantum Language.

#### `match.cpp` / `match.h`

These files include functions for token matching. They identify specific tokens or patterns within the source code and prepare them for further processing.

#### `atEnd.cpp` / `atEnd.h`

These functions determine whether the end of the input source code has been reached. This is crucial for controlling the flow of parsing operations and handling edge cases.

#### `skipNewlines.cpp` / `skipNewlines.h`

These files provide functionality to skip over newline characters in the source code. Newline characters often do not affect the syntax but can complicate parsing logic, so this feature helps streamline the process.

#### `parse.cpp` / `parse.h`

The primary file for parsing the input source code. It orchestrates the entire parsing process, utilizing the functionalities provided by the other files to construct the AST.

## Overall Flow

1. **Initialization**: The parser initializes itself by setting up necessary data structures and reading the input source code.
2. **Tokenization**: The input source code is broken down into tokens using lexical analysis tools.
3. **Syntax Checking**: Each token is checked against predefined syntax rules to ensure its validity.
4. **Token Matching**: Tokens are matched according to their type and value, forming valid sequences.
5. **Skipping Newlines**: The parser skips over any unnecessary newline characters to maintain focus on significant syntax elements.
6. **Parsing**: The parsed tokens are then used to construct the AST through recursive descent parsing or another suitable algorithm.
7. **Completion**: Once the parsing is complete, the AST represents the structured form of the quantum program, ready for further compilation stages.

By following this flow, the `ParserCore` component ensures that the input source code is accurately converted into an AST, providing a solid foundation for the rest of the Quantum Language compiler.