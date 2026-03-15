# QuantumLanguage Compiler - Token.h

## Overview

The `include/Token.h` header file is an essential component of the QuantumLanguage compiler, responsible for defining the structure and behavior of tokens used throughout the compilation process. Tokens represent individual elements in the source code, such as literals, identifiers, keywords, operators, delimiters, and special symbols. This file serves as the foundation for parsing and understanding the syntax of the QuantumLanguage source code.

## Key Design Decisions

### Tokenization Strategy

**Why:** The choice to use a simple enumeration (`TokenType`) for token types allows for clear and efficient categorization of lexical elements. Each token type is explicitly defined, making it easy to understand and implement the lexer.

### String Storage

**Why:** Storing token values as `std::string` provides flexibility and ensures that all token types can be represented accurately. Using move semantics in the constructor helps optimize memory management by transferring ownership of the string rather than copying it.

### Line and Column Tracking

**Why:** Keeping track of the line and column numbers where each token appears aids in error reporting and debugging. It allows the compiler to provide precise location information when encountering syntax errors or other issues.

## Classes and Functions

### TokenType Enum Class

**Purpose:** Defines an enumeration of possible token types, covering literals, identifiers, keywords, operators, delimiters, and special symbols.

**Behavior:** Each token type is associated with a unique identifier and a descriptive name, facilitating easy identification and handling during parsing.

### Token Struct

**Purpose:** Represents a single token in the source code, containing its type, value, line number, and column position.

**Constructor:**
```cpp
Token(TokenType t, std::string v, int ln, int c);
```
- **Parameters:**
  - `t`: The type of the token.
  - `v`: The value of the token.
  - `ln`: The line number where the token appears.
  - `c`: The column position where the token appears.
- **Behavior:** Initializes a new `Token` object with the specified type, value, line number, and column position.

**Function:**
```cpp
std::string toString() const;
```
- **Return Value:** A string representation of the token, useful for debugging and logging purposes.
- **Behavior:** Returns a formatted string that includes the token's type, value, line number, and column position.

## Tradeoffs and Limitations

- **Flexibility vs. Simplicity:** While using `std::string` for token values offers flexibility, it may introduce performance overhead compared to fixed-size data types. However, the simplicity of the implementation outweighs these potential drawbacks.
- **Error Reporting:** Accurate line and column tracking enhances error reporting but requires additional complexity in the lexer and parser.
- **Special Cases:** Handling special cases like template strings and decorators may require additional logic and state management within the lexer.

## Conclusion

The `include/Token.h` header file plays a crucial role in the QuantumLanguage compiler by defining the structure and behavior of tokens. Its design choices ensure clarity, efficiency, and accurate error reporting, forming the basis for effective parsing and semantic analysis of the source code.