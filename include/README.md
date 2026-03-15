# Token.h - Header File for Quantum Language Compiler

## Overview

The `Token.h` header file is an essential component of the Quantum Language compiler, responsible for defining the structure and enumeration of tokens used during lexical analysis. This file serves as the foundation for converting raw text into meaningful units that can be processed further by the parser.

## Key Design Decisions

### Use of Enum Class for Token Types

**WHY:** Using `enum class` ensures that token types are strongly typed and scoped within their own namespace, preventing potential conflicts with other identifiers and enhancing code readability and maintainability.

### String-Based Token Values

**WHY:** Storing token values as strings allows for easy representation of literals, identifiers, and keywords. It also simplifies the process of comparing token values during semantic analysis.

### Line and Column Information

**WHY:** Keeping track of the line and column numbers where each token appears facilitates error reporting and debugging. This information is crucial for providing precise location details when syntax errors occur.

## Classes and Functions Documentation

### TokenType Enum Class

**Purpose:** Defines all possible token types encountered during lexical analysis.

**Behavior:** Each member represents a distinct type of token, such as literals, identifiers, keywords, operators, delimiters, and special symbols like EOF (End of File).

### Token Struct

**Purpose:** Represents a single token in the quantum language source code.

**Behavior:** The `Token` struct contains four main fields:
- `type`: Specifies the type of the token using the `TokenType` enum.
- `value`: Holds the string value associated with the token.
- `line`: Records the line number where the token was found.
- `col`: Records the column number where the token was found.

#### Constructor

```cpp
Token(TokenType t, std::string v, int ln, int c);
```
**Purpose:** Initializes a new `Token` object with the specified type, value, line number, and column number.

**Parameters:**
- `t`: The type of the token (`TokenType`).
- `v`: The string value of the token (`std::string`).
- `ln`: The line number where the token occurs (`int`).
- `c`: The column number where the token occurs (`int`).

#### toString Method

```cpp
std::string toString() const;
```
**Purpose:** Converts the `Token` object to a human-readable string format, which is useful for debugging and logging purposes.

**Return Value:** A string representing the token's type, value, line, and column.

## Tradeoffs and Limitations

### Memory Overhead

Storing token values as strings incurs some memory overhead compared to storing them directly as integers or characters. However, this approach provides flexibility and ease of use, especially when dealing with variable-length literals and identifiers.

### Performance Considerations

While the string-based representation offers convenience, it may impact performance slightly during token comparison and processing. For large-scale projects, optimizing these operations might be necessary.

### Limited Type Safety

Using `enum class` for token types enhances safety but requires explicit casting when performing comparisons or operations on token types. This adds a bit of complexity to the codebase.

## Conclusion

The `Token.h` header file plays a critical role in the Quantum Language compiler by defining the structure and enumeration of tokens. Its design choices aim to balance usability, flexibility, and performance, while also ensuring strong typing and scope management for token types. Although there are some tradeoffs, particularly regarding memory usage and performance, the benefits of clear and maintainable code outweigh these concerns.