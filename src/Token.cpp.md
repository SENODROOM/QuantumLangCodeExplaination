# Token.cpp

## Overview

The `Token.cpp` file is a critical part of the Quantum Language compiler, responsible for handling individual lexical tokens extracted from the input source code. These tokens serve as the building blocks for forming the syntax tree, which is essential for further stages of semantic analysis and code generation.

## Role in Compiler Pipeline

During the lexer stage of the compiler pipeline, `Token.cpp` plays a pivotal role. It converts raw characters into meaningful tokens based on the language's grammar rules. Each token contains information such as its type, value, line number, and column position in the source code. This information is then used by the parser to construct the abstract syntax tree (AST).

## Key Design Decisions and Their Reasons

### 1. Use of `std::ostringstream` for String Conversion

**Why**: The decision to use `std::ostringstream` was made to provide a flexible and efficient way to format token information into a string. This approach allows for easy customization of the output format without modifying the underlying logic of token creation. Additionally, it improves performance by avoiding multiple string concatenations.

### 2. Encapsulation of Token Properties

**Why**: By encapsulating the properties of a token within a single class (`Token`), we ensure data integrity and abstraction. This makes the code more maintainable and easier to extend in the future. Each property (e.g., type, value, line number) is accessed through member functions, providing clear interfaces for interacting with tokens.

## Documented Classes and Functions

### Class: `Token`

#### Purpose

The `Token` class represents a single lexical token in the Quantum Language compiler. It encapsulates all necessary information about a token, including its type, value, line number, and column position.

#### Member Variables

- `int line`: Line number where the token appears.
- `int col`: Column number where the token begins.
- `std::string value`: Value of the token.

#### Member Functions

- `std::string toString() const`: Returns a formatted string representation of the token, useful for debugging and logging purposes.

### Function: `Token::toString() const`

#### Purpose

This function provides a human-readable string representation of a `Token`. It includes the token's value, line number, and column position, making it easier to understand the context in which the token was encountered.

#### Behavior

- Concatenates the token's value, line number, and column position into a single string.
- Formats the string using square brackets and colons to separate different pieces of information.
- Returns the formatted string.

#### Tradeoffs/Limitations

- **Performance**: While `std::ostringstream` is efficient, it may introduce minor overhead compared to simpler string concatenation methods. However, this tradeoff is acceptable given the benefits of flexibility and abstraction.
- **Readability**: The use of `std::ostringstream` enhances readability by clearly separating different parts of the token information. This can be particularly useful when dealing with complex token types or large source files.

## Conclusion

The `Token.cpp` file is a fundamental part of the Quantum Language compiler, responsible for converting raw characters into meaningful lexical tokens. Through careful design choices, such as using `std::ostringstream` for string conversion and encapsulating token properties within a single class, the file ensures data integrity, flexibility, and improved performance. By documenting these components thoroughly, developers can better understand their roles and how they contribute to the overall functionality of the compiler.