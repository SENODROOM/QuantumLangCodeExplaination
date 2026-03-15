# QuantumLanguage Compiler - Parser.h

## Overview

The `include/Parser.h` header file is an essential part of the QuantumLanguage compiler, focusing on the parsing phase of the compilation process. This file defines the `Parser` class, which takes a vector of `Token`s and constructs an Abstract Syntax Tree (AST) representing the structure of the source code. The AST serves as the intermediate representation that subsequent phases of the compiler use to generate machine code or other outputs.

## Key Design Decisions

### Error Handling

The decision to derive `ParseError` from `std::runtime_error` was made to leverage C++'s exception handling mechanism, making it easier to propagate errors up the call stack. The inclusion of line and column numbers in the error message provides more context, aiding in debugging and improving user experience.

### Pratt Parsing Algorithm

Choosing the Pratt parsing algorithm for expression parsing was driven by its ability to handle operator precedence without requiring complex recursive descent parsers. This approach simplifies the implementation and reduces the risk of syntax errors, especially when dealing with nested operators.

## Documentation of Classes and Functions

### Class: ParseError

**Purpose:** Represents an error encountered during parsing, providing additional context about the location of the error.

**Behavior:** Inherits from `std::runtime_error` and adds line and column information to the error message.

**Trade-offs/Limitations:** While exceptions are useful for error handling, they can be expensive in terms of performance and resource usage. Additionally, overuse of exceptions can make the code harder to understand and maintain.

### Class: Parser

**Purpose:** Converts a sequence of `Token`s into an AST, representing the syntactic structure of the source code.

**Behavior:** Initializes with a vector of `Token`s and provides methods to parse different parts of the language, including statements, expressions, and declarations.

**Major Methods:**

- **parse():** Entry point for parsing the entire input.
- **parseStatement(), parseBlock(), parseBodyOrStatement():** Handle different types of statements and blocks.
- **parseVarDecl(), parseFunctionDecl(), parseClassDecl():** Parse variable, function, and class declarations.
- **parseExpr(), parseAssignment(), etc.:** Implement Pratt parsing for various operators and expressions.

**Trade-offs/Limitations:** Pratt parsing is powerful but requires careful management of state and precedence rules. Overly complex precedence rules can lead to bugs and decreased readability.

### Function: current()

**Purpose:** Retrieves the current token being processed.

**Behavior:** Returns the token at the current position (`pos`) in the vector.

### Function: peek(int offset)

**Purpose:** Allows lookahead to see the next token(s) without advancing the parser's position.

**Behavior:** Returns the token at the specified offset relative to the current position.

### Function: consume()

**Purpose:** Advances the parser to the next token.

**Behavior:** Increments the position (`pos`) and returns the previous token.

### Function: expect(TokenType t, const std::string &msg)

**Purpose:** Ensures the current token matches the expected type, throwing an error if not.

**Behavior:** Compares the current token to the expected type and throws a `ParseError` if there is a mismatch.

### Function: isCTypeKeyword(TokenType t) const

**Purpose:** Determines if a given token type represents a C-like type keyword.

**Behavior:** Checks if the token type corresponds to any of the predefined C-type keywords like `int`, `double`, etc.

## Conclusion

The `Parser.h` file plays a critical role in the QuantumLanguage compiler by converting textual input into a structured AST. The choice of error handling and Pratt parsing algorithms reflects a balance between simplicity and robustness. By documenting each class and function thoroughly, we ensure that future developers can easily understand and contribute to the compiler's parsing logic. However, it's important to acknowledge the potential trade-offs associated with these choices, particularly regarding performance and maintainability.