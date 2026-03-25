# ParserCore Component of the Quantum Language Compiler

The `ParserCore` component is a crucial part of the Quantum Language (QL) compiler, responsible for parsing the input source code into an abstract syntax tree (AST). This process involves lexical analysis and syntactic analysis to ensure that the source code adheres to the language's grammar rules.

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

## Files and Functions

### check.cpp/check.h

- **Purpose**: Contains utility functions to verify conditions during parsing.
- **Functions**:
  - `bool check(const std::string& condition)`: Checks if a given condition is met.
  - `void assertCheck(bool condition, const std::string& message)`: Asserts that a condition is true; throws an error if false.

### match.cpp/match.h

- **Purpose**: Implements functions to match specific patterns in the input source code.
- **Functions**:
  - `bool match(const std::string& pattern)`: Attempts to match a given pattern at the current position in the input.
  - `std::string getMatchedText()`: Retrieves the text that was matched.

### atEnd.cpp/atEnd.h

- **Purpose**: Provides functionality to determine if the end of the input has been reached.
- **Functions**:
  - `bool atEnd()`: Returns true if the parser has reached the end of the input source code.

### skipNewlines.cpp/skipNewlines.h

- **Purpose**: Handles the skipping of newline characters in the input source code.
- **Functions**:
  - `void skipNewlines()`: Skips over all consecutive newline characters until a non-newline character is encountered.

### parse.cpp/parse.h

- **Purpose**: The main file containing the implementation of the parsing logic.
- **Classes**:
  - `class Parser`: Manages the state and operations of the parser.
- **Functions**:
  - `Node* parseProgram()`: Parses the entire program from the input source code.
  - `Node* parseStatement()`: Parses individual statements within the program.
  - `Node* parseExpression()`: Parses expressions used within statements.

## Overall Flow

1. **Initialization**: The `Parser` class is initialized with the input source code.
2. **Lexical Analysis**: The `skipNewlines` function is called to handle any leading newlines. Then, the `match` function checks for specific tokens or patterns.
3. **Syntactic Analysis**: The `parseProgram`, `parseStatement`, and `parseExpression` functions recursively build the AST based on the parsed tokens.
4. **Validation**: The `check` function ensures that the parsed structure meets the required criteria before proceeding.
5. **Completion**: The parsing continues until the end of the input is reached, as determined by the `atEnd` function.

By following this structured approach, `ParserCore` efficiently transforms the quantum language source code into a detailed AST, which can then be further processed by subsequent stages of the compiler.