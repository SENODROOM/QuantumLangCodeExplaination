# Parser.cpp - The Heart of Syntax Analysis in Quantum Language Compiler

## Overview

`Parser.cpp` is a critical component of the Quantum Language compiler, responsible for converting a sequence of tokens into an Abstract Syntax Tree (AST). This transformation ensures that the program conforms to the syntax rules of the Quantum Language, facilitating further semantic analysis and code generation stages.

## Role in the Compiler Pipeline

The parser plays a pivotal role in the compilation process by taking the output from the lexer—a stream of tokens—and constructing a structured representation of the program's syntactic elements. This AST serves as the foundation for subsequent phases such as semantic analysis, optimization, and code generation.

## Key Design Decisions

### Token Consumption Strategy

To ensure robust error handling and maintainability, the parser uses a strategy where it consumes tokens one at a time and checks their types against expected values. This approach allows for easy identification and reporting of syntax errors, as well as simplifies the implementation of recursive descent parsing techniques.

### Error Handling

The parser includes a mechanism to handle unexpected token types gracefully. When a token type does not match the expected pattern, a `ParseError` exception is thrown, providing details about the error, including the expected token type and the actual token encountered. This helps in debugging and improving the user experience by giving clear feedback on syntax issues.

### Decorator Parsing

One notable feature of the parser is its ability to handle Python-style decorators. These are skipped during the parsing process, ensuring compatibility with both Quantum Language and existing Python codebases. This decision was made to broaden the scope of the compiler and make it more versatile.

## Documentation of Major Classes/Functions

### Parser Class

**Purpose**: Manages the parsing process, consuming tokens and building an AST.

**Behaviour**:
- Initializes with a vector of tokens.
- Provides methods to access and manipulate the current token position.
- Parses statements and constructs an AST.

### parse() Function

**Purpose**: Entry point for the parsing process, starting from the root block statement.

**Behaviour**:
- Creates a new AST node representing a block.
- Iterates through tokens, parsing individual statements until the end of input is reached.
- Handles skipping of newlines and decorators.

### parseStatement() Function

**Purpose**: Parses a single statement from the token stream.

**Behaviour**:
- Skips any leading newlines.
- Checks for specific keywords (e.g., `let`, `const`) and parses corresponding declarations.
- Supports skipping of storage class specifiers (e.g., `static`, `extern`) common in C/C++.

## Tradeoffs/Limitations

- **Decorator Support**: While allowing for broader compatibility, the parser currently skips all decorators without processing them. Future enhancements may include support for decorator attributes.
- **Storage Class Specifiers**: Although supported, the parser treats these as comments rather than meaningful language elements. Their presence in the AST might be useful for optimizations or code generation but is not utilized in the current implementation.
- **Error Reporting**: Detailed error messages are provided, which can be overwhelming for simple syntax errors. Simplifying error reports could improve usability.

## Usage Example

Here’s a simplified example of how you might use the `Parser` class:

```cpp
#include "Parser.h"

int main() {
    std::vector<Token> tokens = ...; // Assume this is populated with tokens
    Parser parser(tokens);
    try {
        ASTNodePtr ast = parser.parse();
        // Further processing of the AST...
    } catch (const ParseError& e) {
        std::cerr << "Parse error: " << e.what() << std::endl;
    }
    return 0;
}
```

This README provides a comprehensive overview of the `Parser.cpp` file, detailing its role, design decisions, and functionality. It also highlights potential areas for improvement and usage examples to facilitate better understanding and integration into the Quantum Language compiler.