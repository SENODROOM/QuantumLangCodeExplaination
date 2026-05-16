# ParserCore Component of the Quantum Language Compiler

The `ParserCore` component is a vital module within the Quantum Language (QL) compiler, tasked with converting the input source code into an Abstract Syntax Tree (AST). This transformation ensures that the source code adheres strictly to the language's syntactical rules, enabling accurate execution in later stages of compilation.

## Overview

The `ParserCore` component consists of several key functions and classes designed to handle various aspects of parsing quantum source code:

1. **check**: Validates whether the current token matches the expected type or value, ensuring correct syntax.
2. **match**: Consumes the current token if it matches the expected type or value, advancing the parser state.
3. **atEnd**: Determines if the end of the input has been reached, preventing further parsing errors.
4. **skipNewlines**: Skips over any newline characters in the source code, maintaining proper parsing flow.
5. **parse**: The main function that initiates the parsing process, orchestrating the creation of the AST based on the source code structure.

These functions work together to build a robust and efficient parsing system, capable of handling complex quantum programs while ensuring accuracy and reliability.

### Directory Structure

```
parsercore/
├── check.cpp
├── match.cpp
├── atEnd.cpp
├── skipNewlines.cpp
└── parse.cpp
```

Each file corresponds to a specific aspect of the parsing process:

- **check.cpp**: Contains implementations for validating tokens against expected types or values.
- **match.cpp**: Implements logic for consuming tokens and advancing the parser state.
- **atEnd.cpp**: Provides functionality to determine when the end of the input has been reached.
- **skipNewlines.cpp**: Manages the skipping of newline characters in the source code.
- **parse.cpp**: Orchestrates the entire parsing process, utilizing the above functions to construct the AST.

### Overall Flow

The typical flow through the `ParserCore` component involves the following steps:

1. **Initialization**: The parser initializes with the source code and sets up necessary data structures.
2. **Tokenization**: The source code is broken down into individual tokens.
3. **Parsing**: Tokens are processed sequentially, using the `check`, `match`, and other functions to validate and consume them according to the language's grammar rules.
4. **Tree Construction**: As valid tokens are consumed, they are used to construct nodes in the AST, representing different elements of the quantum program.
5. **Completion**: Once all tokens have been processed, the AST is complete, ready for further analysis and optimization.

By following this structured approach, the `ParserCore` component ensures that the quantum source code is accurately parsed and transformed into an AST, forming the foundation for successful compilation and execution.