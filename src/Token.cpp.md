# Token.cpp

## Overview

The `Token.cpp` file is an essential part of the Quantum Language compiler, responsible for representing and manipulating individual lexical tokens extracted from the input source code. These tokens serve as the building blocks of the syntax tree, critical for the subsequent phases of semantic analysis and code generation.

## Role in Compiler Pipeline

During the lexer phase of the compiler pipeline, `Token.cpp` plays a pivotal role. It converts raw characters from the source code into structured token objects that contain information such as the token type, value, line number, and column position. This process ensures that the compiler can accurately parse the source code and build a meaningful syntax tree.

## Key Design Decisions

### Use of `std::ostringstream` for String Formatting

**WHY**: The choice to use `std::ostringstream` for string formatting was driven by its efficiency and flexibility. `std::ostringstream` allows for dynamic construction of strings, which is ideal for creating complex token representations that include various attributes like line number, column, and value. Additionally, it provides better performance compared to concatenating strings using `+`, especially when dealing with multiple attributes.

### Encapsulation of Token Attributes

**WHY**: By encapsulating token attributes within the `Token` class, we ensure data hiding and abstraction. This approach prevents direct manipulation of token properties from outside the class, reducing the risk of errors and improving maintainability. It also allows us to add validation logic or modify how certain attributes are accessed without affecting other parts of the compiler.

## Classes and Functions Documentation

### Class: `Token`

**Purpose**: Represents a single lexical token extracted from the source code.

**Attributes**:
- `line`: Line number where the token appears.
- `col`: Column position where the token begins.
- `value`: The actual text value of the token.

**Functions**:
- `toString() const`: Returns a string representation of the token in the format `[line:col value]`.

### Function: `Token::toString() const`

**Purpose**: Converts the token object into a human-readable string format.

**Behaviour**: Utilizes `std::ostringstream` to construct a string containing the token's line number, column position, and value. This method is particularly useful for debugging and logging purposes, allowing developers to easily understand the structure and content of tokens.

**Tradeoffs/Limitations**:
- **Performance**: While `std::ostringstream` is efficient for most cases, it may introduce minor overhead compared to simpler string concatenation methods. However, this tradeoff is acceptable given the benefits of improved readability and maintainability.
- **Flexibility**: Although `std::ostringstream` offers extensive formatting capabilities, it can be more verbose than necessary for simple token representations. Future enhancements might consider using simpler methods for basic tokens while retaining the powerful formatting features for more complex scenarios.

## Conclusion

The `Token.cpp` file is a fundamental module in the Quantum Language compiler, ensuring accurate tokenization and representation of the source code. Through strategic design choices and robust implementation, it contributes significantly to the overall functionality and reliability of the compiler.