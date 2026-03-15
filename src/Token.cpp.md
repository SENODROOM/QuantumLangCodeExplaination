# Token.cpp

## Overview

The `Token.cpp` file is a crucial part of the Quantum Language compiler, responsible for representing and manipulating individual tokens extracted from the input source code. Tokens serve as the basic building blocks of the syntax tree, essential for further stages like semantic analysis and code generation.

## Key Design Decisions

### Utilization of `std::ostringstream` for String Formatting

**WHY**: Using `std::ostringstream` simplifies string formatting operations. It provides a flexible way to construct strings dynamically, which is particularly useful when dealing with various token types and their associated information. This approach enhances readability and maintainability compared to manual string concatenation.

### Choice of `const` Methods

**WHY**: Declaring member functions as `const` ensures that they do not modify the state of the object. This is important for methods that need to be called on constant objects, such as those used during the construction of the syntax tree. By marking these methods as `const`, we can guarantee that the integrity of the token objects is preserved throughout the compilation process.

## Classes and Functions Documentation

### Class: Token

**Purpose**: Represents a single token in the Quantum Language source code. Each token contains information about its type, value, and location within the source file.

#### Member Variables:

- `line`: The line number where the token appears in the source code.
- `col`: The column number where the token starts in the source code.
- `value`: The actual text value of the token.
- `type`: The type of the token (e.g., keyword, identifier, operator).

#### Member Functions:

- **Constructor (`Token(int line, int col, const std::string& value, TokenType type`)**:
  - Initializes a new `Token` object with the specified line number, column number, value, and type.

- **Destructor (`~Token()`)**:
  - Cleans up any resources held by the `Token` object.

- **Method (`std::string toString() const`)**:
  - Returns a string representation of the token in the format `[line:col value]`. This method uses `std::ostringstream` for efficient string formatting.

### Function: `std::string Token::toString() const`

**Purpose**: Converts the `Token` object into a human-readable string format.

**Behavior**:
- Constructs a formatted string using `std::ostringstream`.
- The format includes the line number, column number, and the token's value.
- Returns the formatted string.

**Trade-offs/Limitations**:
- While `std::ostringstream` provides flexibility and efficiency, it may introduce overhead if used excessively.
- The function assumes that the `Token` object is valid and properly initialized.

## Usage Example

Here's how you might use the `Token` class in your compiler:

```cpp
#include "Token.h"

int main() {
    Token token(1, 5, "example", TokenType::IDENTIFIER);
    std::cout << "Token: " << token.toString() << std::endl;
    return 0;
}
```

This example creates a `Token` object representing an identifier named "example" at line 1, column 5, and prints its string representation.

## Conclusion

The `Token.cpp` file plays a vital role in the Quantum Language compiler by providing a robust and efficient mechanism for handling individual tokens. Through the strategic use of `std::ostringstream` and `const` methods, the file ensures both performance and safety, making it an indispensable part of the compiler's architecture.