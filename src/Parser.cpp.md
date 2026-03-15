# Parser.cpp - The Heart of Syntax Analysis in Quantum Language Compiler

## Overview

`Parser.cpp` is a vital component of the Quantum Language compiler, tasked with transforming a stream of tokens into an Abstract Syntax Tree (AST). This conversion ensures the program adheres to the syntax rules of the Quantum Language, enabling subsequent semantic analysis and code generation phases.

## Key Design Decisions

### Tokenization vs Lexical Analysis

**WHY:** Tokenization is the initial stage in any compiler, breaking down the input source code into discrete units called tokens. This separation allows the parser to focus solely on the syntactic structure without being concerned about lexical details such as whitespace and comments. By treating tokenization as a separate step, `Parser.cpp` can efficiently manage the flow of tokens during parsing, enhancing modularity and maintainability.

## Documentation of Major Classes and Functions

### Class: Parser

**Purpose:** The `Parser` class encapsulates the logic for converting a sequence of tokens into an AST. It manages the parsing process, including error handling and skipping unnecessary tokens.

**Behavior:**
- **Constructor:** Initializes the parser with a vector of tokens.
- **Current Token:** Returns the current token being processed.
- **Peek Token:** Returns the token at a specified offset relative to the current position.
- **Consume Token:** Advances the parser's position to the next token and returns it.
- **Expect Token:** Verifies if the current token matches the expected type; throws an exception if not.
- **Check Token:** Checks if the current token matches a specific type.
- **Match Token:** Consumes the current token if it matches the specified type; returns `false` otherwise.
- **At End:** Determines if the parser has reached the end of the token stream.
- **Skip Newlines:** Skips all newline tokens until the end of the stream or a non-newline token is encountered.
- **Parse:** Begins the parsing process, constructing an AST from the token stream.
- **Parse Statement:** Parses a single statement from the token stream, handling various types of statements including variable declarations and expressions.

### Function: parse()

**Purpose:** The primary function of the `Parser` class, `parse()`, initiates the parsing process by creating a root `BlockStmt` node and populating it with statements parsed from the token stream.

**Behavior:**
- Creates a unique pointer to a `BlockStmt` node.
- Iteratively parses statements using `parseStatement()` and appends them to the block's list of statements.
- Skips newlines between statements to ensure proper parsing.

### Function: parseStatement()

**Purpose:** Parses a single statement from the token stream, identifying and processing different types of statements.

**Behavior:**
- Skips newlines before processing the statement.
- Handles Python-style decorators by consuming them and their arguments.
- Skips C/C++ storage class specifiers like `static`, `extern`, etc., allowing them to precede variable declarations.
- Identifies and processes variable declarations (`LET` and `CONST`) and other statements based on the current token type.

## Tradeoffs and Limitations

- **Python-like Decorators:** While `Parser.cpp` supports Python-style decorators, it may not handle all edge cases, particularly those involving complex argument lists or nested decorators.
- **C/C++ Storage Specifiers:** Although these specifiers are skipped, they might interfere with the intended use of variables in certain contexts, requiring careful consideration during semantic analysis.
- **Error Handling:** Basic error handling is implemented through exceptions, which may need refinement for more sophisticated error reporting and recovery mechanisms.

## Conclusion

`Parser.cpp` plays a critical role in the Quantum Language compiler by converting tokenized input into an AST. Through strategic design decisions, it ensures efficient parsing and robust error handling, setting the foundation for further stages of compilation. Despite some limitations, its modular approach facilitates easy maintenance and extension.