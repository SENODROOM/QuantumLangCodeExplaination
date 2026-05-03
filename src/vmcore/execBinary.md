# `execBinary`

The `execBinary` function is a critical component of the Quantum Language compiler's virtual machine core (`VmCore.cpp`). Its primary responsibility is to execute binary operations between two quantum values (`L` and `R`) based on the specified operation type (`op`). This function ensures that operations can be performed seamlessly across different data types, including strings, arrays, and numbers.

## Functionality

### String Concatenation
- **Condition**: When `op` is `Op::ADD` and either `L` or `R` is a string.
- **Operation**: The function concatenates the string representations of `L` and `R`.
- **Return Value**: A new `QuantumValue` containing the concatenated string.

### Array Concatenation
- **Condition**: When `op` is `Op::ADD` and both `L` and `R` are arrays.
- **Operation**: The function creates a new array by copying all elements from `L` followed by all elements from `R`.
- **Return Value**: A new `QuantumValue` containing the concatenated array.

### Comparison Operators
- **Condition**: When `op` is either `Op::EQ` (equal) or `Op::NEQ` (not equal).
- **Operation**: The function compares the values of `L` and `R`, allowing for mixed types.
  - For equality (`Op::EQ`), it returns `true` if the values are equal, otherwise `false`.
  - For inequality (`Op::NEQ`), it returns `true` if the values are not equal, otherwise `false`.
- **Return Value**: A new `QuantumValue` containing a boolean result of the comparison.

### Numeric Arithmetic
- **Condition**: When `op` is any numeric arithmetic operation (`Op::ADD`, `Op::SUB`, `Op::MUL`).
- **Operation**:
  - Converts `L` and `R` to their numeric equivalents if they are not already numbers.
  - Handles special cases where one or both operands are strings or booleans by attempting to convert them to numbers.
  - Performs the specified arithmetic operation (`ADD`, `SUB`, `MUL`).
- **Return Value**: A new `QuantumValue` containing the result of the arithmetic operation as a number.

## Parameters and Return Value

- **Parameters**:
  - `L`: The left-hand side quantum value.
  - `R`: The right-hand side quantum value.
  - `op`: The operation type to perform, which can be one of `Op::ADD`, `Op::SUB`, `Op::MUL`, `Op::EQ`, or `Op::NEQ`.

- **Return Value**:
  - Returns a new `QuantumValue` representing the result of the binary operation.

## Edge Cases

- **Mixed Types in Comparison**: The function correctly handles comparisons between different types by converting them to numbers when necessary.
- **Non-Numeric Strings in Arithmetic**: If a string cannot be converted to a number, it defaults to `0`. This allows operations like `"5" + 2` to produce `"52"` instead of throwing an error.
- **Negative Numbers in Array Repeat**: When using the multiplication operator (`Op::MUL`) with arrays and negative numbers, the function treats the absolute value of the number and repeats the array accordingly.

## Interactions with Other Components

- **Type Conversion**: The function interacts with the `isNumber()`, `isString()`, `isBool()`, `asNumber()`, `asString()`, and `asBool()` methods of the `QuantumValue` class to determine and convert the types of `L` and `R`.
- **Array Operations**: For array operations, the function uses the `std::shared_ptr<Array>` and related methods to manage and manipulate arrays efficiently.
- **Error Handling**: The function includes error handling mechanisms to gracefully handle situations where string conversion fails, ensuring robustness in the face of unexpected input.

Overall, the `execBinary` function serves as a versatile and essential part of the Quantum Language compiler's virtual machine, enabling complex operations across various data types with ease and reliability.