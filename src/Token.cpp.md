# Token.cpp

## Overview

The `Token.cpp` file is a critical part of the Quantum Language compiler, responsible for representing and manipulating individual lexical tokens extracted from the input source code. These tokens serve as the building blocks of the syntax tree, essential for subsequent stages of semantic analysis and code generation.

## Role in Compiler Pipeline

This file plays a pivotal role in the lexer phase of the compiler. It defines how tokens are converted into strings, which aids in debugging and error reporting. The `Token` class encapsulates the properties of a token, such as its type, value, line number, and column position, providing a structured way to handle these elements throughout the compilation process.

## Key Design Decisions

### Utilization of `std::ostringstream` for String Formatting

**WHY**: Using `std::ostringstream` simplifies the process of string formatting. This approach enhances readability and maintainability by allowing complex string constructions through method chaining rather than manual concatenation. Additionally, it improves performance by reducing the overhead associated with multiple string concatenations.

## Documentation of Major Classes/Functions

### Class: `Token`

**Purpose**: Represents a single lexical token in the Quantum Language source code.

**Behavior**:
- **Constructor**: Initializes a token with its type, value, line number, and column position.
- **Destructor**: Cleans up any resources held by the token object.
- **Function: `toString()`**
  - **Purpose**: Converts the token's properties into a human-readable string format.
  - **Behavior**: Returns a string representation of the token in the format `[line:col value]`. For example, `[10:5 identifier myVar]`.
  - **Tradeoffs**: While this method provides a clear and concise string representation, it may not be suitable for all use cases where more complex formatting is required.

### Function: `Token::toString() const`

**Purpose**: Converts the token's properties into a formatted string.

**Behavior**:
- Takes no parameters.
- Uses `std::ostringstream` to construct a string in the format `[line:col value]`.
- Returns the constructed string.

**Tradeoffs**: This function offers simplicity and readability but might lack flexibility for advanced formatting needs.

## Tradeoffs/Limitations

- **Simplicity vs. Flexibility**: The choice of using `std::ostringstream` for string formatting sacrifices some flexibility in favor of ease of use and performance.
- **Resource Management**: Although the destructor cleans up resources, the current implementation assumes that no dynamic memory is allocated within the `Token` class itself. If dynamic memory management is introduced, proper resource handling must be implemented in both the constructor and destructor.

## Conclusion

The `Token.cpp` file is a fundamental piece of the Quantum Language compiler, ensuring that lexical tokens are accurately represented and manipulated. By leveraging `std::ostringstream`, the file achieves a balance between simplicity and performance, making it an efficient choice for the lexer phase. However, developers should be aware of the tradeoffs involved when choosing this method for string formatting.