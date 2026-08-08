# ParserCore Component of the Quantum Language Compiler

## Overview

The `ParserCore` component plays a crucial role in the Quantum Language (QL) compiler by converting input source code into an Abstract Syntax Tree (AST). This transformation ensures that the source code strictly conforms to the language's syntactical rules, facilitating efficient processing in subsequent compilation phases.

### Key Functions and Files

- **check**: Validates whether the current token matches a specific expected type.
- **match**: Consumes the current token if it matches the specified type, otherwise throws an error.
- **atEnd**: Determines if the end of the input has been reached.
- **skipNewlines**: Skips over any newline characters encountered during parsing.
- **parse**: The primary function that initiates the parsing process, constructing the AST from the source code.

These functions work together to ensure smooth and accurate parsing of the Quantum Language source code.

## Directory Structure

```
parsercore/
├── check.cpp
├── match.cpp
├── atEnd.cpp
├── skipNewlines.cpp
└── parse.cpp
```

Each file contains the implementation of one of the key functions described above.

## Overall Flow

1. **Initialization**: The parser initializes with the source code and sets up necessary data structures for tracking the current position and state.
2. **Parsing Loop**: The `parse` function enters a loop where it repeatedly calls other functions to consume tokens and construct the AST.
3. **Token Validation**: During parsing, the `check` function is used to validate the current token against expected types.
4. **Token Consumption**: If the token is valid, the `match` function consumes it; otherwise, an error is thrown.
5. **Whitespace Handling**: The `skipNewlines` function handles any whitespace or newline characters encountered during parsing.
6. **Termination Check**: The `atEnd` function checks if the end of the input has been reached, allowing the parser to terminate gracefully.

By following this structured approach, `ParserCore` efficiently transforms the source code into a well-formed AST, ensuring the foundation for further compilation steps.