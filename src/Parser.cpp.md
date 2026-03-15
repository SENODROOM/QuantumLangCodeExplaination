# Parser.cpp - The Heart of Syntax Analysis in Quantum Language Compiler

## Overview

`Parser.cpp` is a crucial component of the Quantum Language compiler, responsible for converting a sequence of tokens into an Abstract Syntax Tree (AST). This transformation ensures that the program conforms to the syntax rules of the Quantum Language, facilitating further semantic analysis and code generation stages.

## Role in the Compiler Pipeline

The parser plays a pivotal role in the compilation process by taking the output of the lexer (tokenized input) and constructing a structured representation of the code. This AST serves as the foundation for subsequent stages such as semantic analysis and code generation.

## Key Design Decisions

### Tokenization vs Lexical Analysis

**WHY:** Tokenization is the initial stage in breaking down the source code into individual lexical elements (tokens), whereas lexical analysis involves understanding the structure and meaning of these tokens. By separating these concerns, `Parser.cpp` can focus on the syntactic correctness of the code without delving into its semantics.

### Handling Decorators and Storage Class Specifiers

**WHY:** To maintain compatibility with existing programming paradigms, it was decided to allow Python-like decorators (e.g., `@property`, `@dataclass`) and C/C++ storage class specifiers (e.g., `static`, `extern`). These features enable developers to use familiar patterns while writing quantum programs, enhancing readability and maintainability.

## Documentation of Major Classes and Functions

### Parser Class

**Purpose:** The `Parser` class is the main driver for parsing the tokenized input. It manages the position within the token list and provides methods for consuming tokens, checking their types, and skipping newlines.

**Behavior:**
- **Constructor:** Initializes the parser with a vector of tokens.
- **current():** Returns the current token being processed.
- **peek(offset):** Returns the token at the specified offset ahead in the token list.
- **consume():** Advances the parser's position and returns the current token.
- **expect(t, msg):** Consumes the next token if it matches the expected type, otherwise throws a `ParseError`.
- **check(t):** Checks if the current token matches the specified type.
- **match(t):** Attempts to match and consume the next token if it matches the specified type.
- **atEnd():** Determines if the parser has reached the end of the token list.
- **skipNewlines():** Skips any newline tokens encountered during parsing.
- **parse():** Parses the entire input, constructing an AST starting with a block statement.
- **parseStatement():** Parses individual statements, handling various syntax constructs including variable declarations and expressions.

### Tradeoffs and Limitations

- **Flexibility vs Simplicity:** Allowing both Python-like decorators and C/C++ storage class specifiers increases the flexibility of the language but adds complexity to the parser.
- **Error Handling:** The parser uses exceptions (`throw ParseError(...)`) to handle syntax errors, which simplifies error reporting but may not be suitable for all environments.
- **Performance:** The recursive descent approach used in the parser can lead to performance issues for deeply nested structures, although this is mitigated by careful optimization.

## Usage Example

To use the `Parser.cpp` module, follow these steps:

1. **Tokenize Input:** Use the lexer to tokenize your source code.
2. **Create Parser Instance:** Pass the tokenized input to the `Parser` constructor.
3. **Parse Code:** Call the `parse()` method to construct the AST.

```cpp
#include "Lexer.h"
#include "Parser.h"

int main() {
    std::string sourceCode = "let x = 42;";
    Lexer lexer(sourceCode);
    std::vector<Token> tokens = lexer.tokenize();

    Parser parser(tokens);
    ASTNodePtr ast = parser.parse();

    // Further processing of the AST...
    return 0;
}
```

This example demonstrates how to integrate `Parser.cpp` with the lexer to parse a simple quantum program.