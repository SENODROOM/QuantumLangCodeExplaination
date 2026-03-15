# Token.cpp

## Overview

The `Token.cpp` file is part of the Quantum Language compiler's core infrastructure. It contains the implementation for the `Token` class, which represents individual tokens parsed from the input source code. Tokens serve as the basic building blocks for the syntax tree and are essential for semantic analysis and code generation phases of the compilation process.

## Design Decisions

### Use of `std::ostringstream` for String Formatting

**WHY**: The choice to use `std::ostringstream` over other string formatting methods like `sprintf` was driven by safety and flexibility. `std::ostringstream` provides a safer way to handle string concatenation, preventing buffer overflow issues that can arise with `sprintf`. Additionally, it offers more control over formatting, allowing for easy adjustments and extensions without risking errors.

### Const Correctness

**WHY**: Making the `toString()` method `const` ensures that it can be called on constant instances of the `Token` class. This is particularly useful during various stages of the compilation process where tokens might need to be accessed without altering their state. By adhering to const correctness, we maintain the integrity of the data and enable better optimization by the compiler.

## Documentation

### Class: `Token`

**Purpose**: Represents an individual token parsed from the input source code.

#### Public Methods:

- **`std::string toString() const`**
  - **Purpose**: Converts the token into a human-readable string representation.
  - **Behaviour**: Returns a string formatted as `[line:col value]`, where `line` and `col` are the line number and column position of the token, respectively, and `value` is the actual text content of the token.
  - **Trade-offs**: Using `std::ostringstream` for string formatting introduces additional overhead compared to simpler methods. However, this overhead is negligible for typical token sizes and provides significant benefits in terms of safety and flexibility.

### Function: `Token::toString() const`

**Purpose**: Converts the token into a human-readable string representation.

**Parameters**: None

**Return Type**: `std::string`

**Description**: This function formats the token's attributes (`line`, `col`, and `value`) into a single string using `std::ostringstream`. The resulting string follows the format `[line:col value]`.

**Example Usage**:
```cpp
Token tok(10, 5, "int");
std::cout << tok.toString(); // Outputs: [10:5 int]
```

## Limitations

- **Performance Overhead**: While `std::ostringstream` is safe and flexible, it may introduce some performance overhead due to dynamic memory allocation and copying. For extremely high-performance applications, a hand-rolled solution might be preferable.
- **Memory Usage**: Creating multiple temporary strings using `std::ostringstream` can lead to increased memory usage, especially if the tokens are numerous or large in size.

Overall, the `Token.cpp` file plays a crucial role in the Quantum Language compiler by providing a robust and safe mechanism for representing and manipulating parsed tokens. Its design choices balance safety, flexibility, and performance considerations, making it suitable for both small-scale and large-scale projects.