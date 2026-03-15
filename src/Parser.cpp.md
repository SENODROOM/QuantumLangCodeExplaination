# Parser.cpp - The Heart of Syntax Analysis in Quantum Language Compiler

## Overview

`Parser.cpp` is a critical component of the Quantum Language compiler, responsible for converting a sequence of tokens into an Abstract Syntax Tree (AST). This transformation ensures that the program conforms to the syntax rules of the Quantum Language, facilitating further semantic analysis and code generation stages.

## Role in the Compiler Pipeline

The parser plays a pivotal role in the compilation process by taking the output of the lexer (tokens) and constructing a structured representation of the program's syntactic elements. It processes these tokens sequentially, building up the AST as it goes. The parsed AST can then be used by subsequent phases of the compiler to perform deeper analysis and generate executable code.

## Key Design Decisions

### Token Stream Management

- **Token Vector**: The parser operates on a vector of `Token` objects, which allows efficient access and manipulation of tokens.
- **Position Tracking**: A `pos` variable keeps track of the current position in the token stream, enabling sequential parsing.

### Error Handling

- **ParseError Exception**: Custom exceptions (`ParseError`) are thrown when syntax errors are encountered, providing clear error messages along with line and column information.
- **Early Termination**: Upon encountering a syntax error, the parser immediately terminates, ensuring that any subsequent processing is not performed on invalid input.

### Flexibility and Extensibility

- **Decorator Support**: The parser includes logic to handle Python-style decorators, allowing for future extensions without modifying core parsing mechanisms.
- **Storage Class Specifiers**: It supports C/C++ style storage class specifiers such as `static`, `extern`, etc., making the parser more versatile and capable of handling various language features.

## Documentation of Major Classes/Functions

### Parser Class

**Purpose**: Manages the parsing of the token stream into an AST.

**Behaviour**:
- Initializes with a vector of tokens.
- Provides methods to access and manipulate the current token and peek ahead.
- Consumes tokens and checks their types.
- Parses statements and constructs the AST.

### parse() Function

**Purpose**: Main entry point for parsing the entire token stream.

**Behaviour**:
- Creates a root `BlockStmt` node.
- Iteratively parses statements until the end of the token stream is reached.
- Skips newlines between statements.

### parseStatement() Function

**Purpose**: Parses individual statements from the token stream.

**Behaviour**:
- Handles Python-style decorators by skipping them.
- Skips C/C++ storage class specifiers.
- Dispatches based on the type of the current token to parse different kinds of statements (e.g., variable declarations).

### skipNewlines() Function

**Purpose**: Skips over newline tokens.

**Behaviour**:
- Continuously consumes newline tokens until a non-newline token is encountered.

### expect() Function

**Purpose**: Ensures the current token matches the expected type.

**Behaviour**:
- Throws a `ParseError` if the current token does not match the expected type.
- Consumes the token if it matches.

### match() Function

**Purpose**: Checks if the current token matches the given type and advances if it does.

**Behaviour**:
- Returns `true` if the current token matches the given type and advances the position.
- Returns `false` otherwise.

### atEnd() Function

**Purpose**: Determines if the end of the token stream has been reached.

**Behaviour**:
- Returns `true` if the current token is an EOF token.
- Returns `false` otherwise.

## Tradeoffs or Limitations

- **Single-Pass Parsing**: The parser currently implements a single-pass approach, which may limit its ability to handle complex grammatical structures or recover from errors gracefully.
- **Language-Specific Features**: While the parser supports some common language features like decorators and storage class specifiers, it may need adjustments for other specific language features or dialects.
- **Performance**: Sequentially accessing tokens and building the AST can be memory-intensive and slow for large programs. Optimizations could be explored to improve performance.

This README provides a comprehensive overview of `Parser.cpp`, detailing its role, design decisions, and functionality.