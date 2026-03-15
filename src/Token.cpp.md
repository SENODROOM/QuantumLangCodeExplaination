# Token.cpp

## Overview

The `Token.cpp` file is an integral component of the Quantum Language compiler, focusing on the representation and manipulation of individual tokens derived from the input source code. These tokens form the fundamental elements of the syntax tree, pivotal for subsequent stages such as semantic analysis and code generation.

## Key Design Decisions

### Utilization of `std::ostringstream` for String Formatting

**WHY**: Employing `std::ostringstream` facilitates efficient string formatting within the `Token` class. This approach allows for easy concatenation of various token attributes (line number, column number, and value) into a single formatted string, enhancing readability and maintainability of the code.

## Documentation of Classes and Functions

### Class: Token

**Purpose**: Represents an individual token parsed from the input source code. Each token encapsulates metadata about its position in the source code and its value.

**Major Functions**:

- **Constructor (`Token(int line, int col, const std::string& value)`)**: Initializes a new token with specified line number, column number, and value.
- **Destructor (`~Token()`)**: Cleans up any resources associated with the token instance.
- **Function: `toString() const`**
  - **Purpose**: Converts the token into a human-readable string format.
  - **Behaviour**: Returns a string representing the token in the format `[<line>:<col> <value>]`, where `<line>` and `<col>` denote the line and column numbers respectively, and `<value>` represents the token's value.
  - **Tradeoffs**: Using `std::ostringstream` ensures flexibility in string formatting but may introduce slight performance overhead compared to direct string concatenation. However, this tradeoff is generally negligible given the simplicity and clarity it provides.

## Tradeoffs and Limitations

- **Performance Overhead**: The use of `std::ostringstream` introduces a minor performance cost due to dynamic memory allocation and copying operations involved in string construction. For high-performance critical applications, this might be a concern.
- **Resource Management**: While `std::ostringstream` manages resources internally, improper usage could lead to resource leaks if not handled correctly. However, in this context, the destructor ensures proper cleanup.

## Conclusion

The `Token.cpp` file plays a crucial role in the Quantum Language compiler by providing a structured way to represent and manipulate tokens. Its design decisions, particularly the choice of `std::ostringstream` for string formatting, enhance both readability and functionality without significant performance penalties. By thoroughly documenting each class and function, developers can better understand and utilize the components effectively.