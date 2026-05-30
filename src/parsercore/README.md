# ParserCore Component of the Quantum Language Compiler

## Overview

The `ParserCore` component is a vital module within the Quantum Language (QL) compiler, tasked with transforming input source code into an Abstract Syntax Tree (AST). This conversion ensures that the source code adheres strictly to the language's syntactical rules, thereby enabling efficient processing in subsequent stages of the compilation pipeline and improving error detection capabilities.

## Files and Functions

### check
- **Purpose**: Validates whether the current token matches the expected type or value.
- **Usage**: Used throughout the parsing process to ensure tokens align with the grammar rules.

### match
- **Purpose**: Consumes the current token if it matches the specified type or value; otherwise, throws an error.
- **Usage**: Ensures that the parsing process progresses only when valid tokens are encountered.

### atEnd
- **Purpose**: Determines if the end of the input stream has been reached.
- **Usage**: Used to control the termination condition of the parsing loop.

### skipNewlines
- **Purpose**: Skips over any newline characters in the input stream.
- **Usage**: Helps maintain consistent parsing behavior across different platforms.

### parse
- **Purpose**: The main function that initiates the parsing process, invoking various helper functions to construct the AST.
- **Usage**: Orchestrates the entire parsing workflow from start to finish.

## Overall Flow

1. **Initialization**: The `ParserCore` is initialized with the input source code.
2. **Tokenization**: The source code is broken down into individual tokens using a tokenizer.
3. **Parsing Loop**:
   - The `parse` function begins the loop, calling `check`, `match`, and other helper functions as needed.
   - Each call to `check` verifies the current token against the expected syntax.
   - If the token matches, `match` consumes it; otherwise, an error is thrown.
   - `skipNewlines` ensures that the parser skips over any unnecessary whitespace.
4. **Tree Construction**: As valid tokens are matched, they are used to build nodes in the AST.
5. **Termination**: The loop continues until `atEnd` indicates that the end of the input stream has been reached.
6. **Output**: The constructed AST is returned, ready for further compilation steps.

By following this structured approach, `ParserCore` ensures robust and accurate parsing of Quantum Language source code, setting the foundation for successful compilation.