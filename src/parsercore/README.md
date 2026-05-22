# ParserCore Component of the Quantum Language Compiler

## Overview

The `ParserCore` component is a fundamental module in the Quantum Language (QL) compiler, designed to convert input source code into an Abstract Syntax Tree (AST). This transformation is essential for ensuring that the source code adheres strictly to the language's syntactical rules, thereby enabling accurate and efficient execution during subsequent compilation phases.

## Files and Functions

### check

- **Purpose**: Validates whether the current token matches the expected type.
- **Parameters**:
  - `expectedType`: The type of token expected.
- **Returns**: A boolean indicating whether the current token matches the expected type.

### match

- **Purpose**: Consumes the current token if it matches the expected type.
- **Parameters**:
  - `expectedType`: The type of token expected.
- **Returns**: A boolean indicating whether the current token was successfully consumed.

### atEnd

- **Purpose**: Determines if the end of the input stream has been reached.
- **Parameters**: None.
- **Returns**: A boolean indicating whether the end of the input stream has been reached.

### skipNewlines

- **Purpose**: Skips over any newline characters in the input stream.
- **Parameters**: None.
- **Returns**: None.

### parse

- **Purpose**: Initiates the parsing process, converting the input source code into an AST.
- **Parameters**: None.
- **Returns**: A pointer to the root node of the generated AST.

## Directory Structure

```
parsercore/
├── include/
│   ├── parsercore.hpp
│   └── ...
├── src/
│   ├── parsercore.cpp
│   └── ...
└── tests/
    ├── test_parsercore.cpp
    └── ...
```

### include/

Contains header files defining the public interface of the `ParserCore` component.

- `parsercore.hpp`: Main header file for the `ParserCore` component.

### src/

Contains source files implementing the functionality of the `ParserCore` component.

- `parsercore.cpp`: Source file containing the implementation of the core parsing logic.

### tests/

Contains unit tests for verifying the correctness of the `ParserCore` component.

- `test_parsercore.cpp`: Unit test file for testing various aspects of the parsing functionality.

## Overall Flow

1. **Initialization**: The `ParserCore` component initializes with the input source code.
2. **Tokenization**: The input source code is tokenized into individual tokens.
3. **Parsing**: Tokens are parsed according to the defined grammar rules, constructing an AST.
4. **Validation**: The AST is validated to ensure it conforms to the language's syntactical rules.
5. **Output**: The valid AST is outputted for further processing by the compiler.

By following this structured approach, the `ParserCore` component ensures robust and reliable parsing of Quantum Language source code, setting the foundation for successful compilation.