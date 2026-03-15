# Parser.cpp - The Heart of Syntax Analysis in Quantum Language Compiler

## Overview

`Parser.cpp` is a crucial component of the Quantum Language compiler, responsible for converting a sequence of tokens into an Abstract Syntax Tree (AST). This transformation ensures that the program conforms to the syntax rules of the Quantum Language, facilitating further semantic analysis and code generation stages.

## Role in the Compiler Pipeline

The parser plays a pivotal role in the compilation process by taking the output of the lexer (tokenized input) and constructing a structured representation of the program's syntax. It handles the recursive descent parsing technique, breaking down the token stream into higher-level constructs such as statements, expressions, and declarations.

## Key Design Decisions

### Recursive Descent Parsing

Recursive descent parsing was chosen due to its simplicity and ease of implementation. Each grammar rule corresponds directly to a function in the parser, making it straightforward to understand and debug. This approach also allows for easy extension and modification of the language's grammar without significant changes to the core parsing logic.

### Token Stream Management

Using a token stream (`std::vector<Token>`) provides efficient access to tokens at any position, allowing the parser to look ahead and make decisions based on upcoming tokens. This is particularly useful for handling complex syntax structures like decorators and storage class specifiers.

### Error Handling

The parser includes robust error handling mechanisms, throwing `ParseError` exceptions when unexpected tokens are encountered. This ensures that the compiler can quickly identify and report syntax errors, improving developer productivity.

## Documentation of Major Classes/Functions

### Parser Class

**Purpose**: Manages the parsing process, converting a sequence of tokens into an AST.

**Behaviour**:
- Initializes with a vector of tokens.
- Provides methods to access and manipulate the current token and peek at future tokens.
- Consumes tokens as they are processed.
- Parses the entire input into a single AST node representing the entire program.

### parse() Function

**Purpose**: Entry point for parsing the entire input.

**Behaviour**:
- Creates a new AST node for the block statement.
- Continuously parses statements until the end of the input is reached.
- Ignores newlines between statements.

### parseStatement() Function

**Purpose**: Parses individual statements from the token stream.

**Behaviour**:
- Skips over any leading newlines or decorators.
- Handles different types of statements (e.g., variable declarations).
- Consumes tokens as they are parsed.

### parseVarDecl(bool mutableFlag) Function

**Purpose**: Parses variable declarations, including optional mutability.

**Behaviour**:
- Consumes the `let` keyword.
- Parses the variable identifier.
- Optionally consumes the `mut` keyword to indicate mutability.
- Parses the variable type and initial value (if present).

## Tradeoffs/Limitations

- **Flexibility vs. Simplicity**: While recursive descent parsing is simple, it may not be the most flexible approach for very complex grammars. However, for Quantum Language, which has a relatively straightforward syntax, this tradeoff is acceptable.
- **Performance**: Token stream management using vectors is generally efficient, but for extremely large inputs, additional optimizations might be necessary.
- **Error Reporting**: The current error reporting mechanism is basic and throws exceptions. For larger projects, more sophisticated error messages and recovery strategies might be beneficial.

This README provides a comprehensive overview of `Parser.cpp`, detailing its role, key design decisions, and documentation of its major components.