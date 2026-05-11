# `execBinary`

The `execBinary` function is an essential part of the Quantum Language compiler's virtual machine core (`VmCore.cpp`). Its main purpose is to execute binary operations between two quantum values (`L` and `R`) based on the specified operation type (`op`). This function handles various types of binary operations including string concatenation, array concatenation, comparison, and numeric arithmetic.

## What It Does

The `execBinary` function takes three parameters:
- `Op op`: The type of binary operation to be executed.
- `QuantumValue L`: The left operand.
- `QuantumValue R`: The right operand.

It returns a `QuantumValue` representing the result of the binary operation.

## Why It Works This Way

### String Concatenation

When both operands are strings or one of them is a string and the other is a number, the function performs string concatenation. If either operand is a string, it attempts to convert the other operand to a string using `std::to_string`. If successful, it concatenates the two strings and returns the result as a new `QuantumValue`.

### Array Concatenation

For array operands, the function checks if both are arrays. If so, it creates a copy of the first array and appends all elements from the second array to it. The resulting array is then returned as a new `QuantumValue`.

### Comparison Operators

Comparison operators (`EQ` and `NEQ`) allow for mixed types. The function converts both operands to their numeric equivalents (if possible) using `std::stod` for strings and `static_cast<double>` for booleans. It then compares these numeric values and returns a boolean `QuantumValue` indicating whether the condition is true or false.

### Numeric Arithmetic

For numeric operands, the function performs basic arithmetic operations such as addition (`ADD`), subtraction (`SUB`), and multiplication (`MUL`). If one of the operands is a string that can be converted to a number, the function attempts the conversion before performing the operation. For array and number combinations in multiplication, the function repeats the array elements according to the number value.

## Parameters/Return Value

- **Parameters**:
  - `Op op`: Specifies the type of binary operation.
  - `QuantumValue L`: The left operand.
  - `QuantumValue R`: The right operand.

- **Return Value**:
  - Returns a `QuantumValue` representing the result of the binary operation.

## Edge Cases

- **Mixed Types**: When comparing or performing arithmetic with mixed types (e.g., string and number), the function ensures proper handling by converting strings to numbers where applicable.
- **Invalid Conversion**: If a string cannot be converted to a number, the function defaults to zero for that operand during arithmetic operations.
- **Negative Numbers**: In numeric operations, negative numbers are handled correctly, allowing for standard mathematical calculations.

## Interactions With Other Components

The `execBinary` function interacts with several other components within the Quantum Language compiler:

- **Type Checking**: It uses methods like `isString()`, `isArray()`, and `isNumber()` to determine the type of each operand.
- **Conversion Functions**: Methods such as `toString()`, `asArray()`, and `asNumber()` are used to manipulate and access the data within `QuantumValue`.
- **Utility Functions**: Helper functions like `valuesEqual()` are utilized for comparisons.

Overall, the `execBinary` function plays a crucial role in executing binary operations efficiently and correctly across different data types within the Quantum Language compiler's virtual machine.