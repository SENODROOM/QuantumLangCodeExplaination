# `execUnary`

The `execUnary` function in the Quantum Language compiler's virtual machine core (`VmCore.cpp`) is designed to execute unary operations on quantum values. This function supports various unary operators including negation (`-`), logical NOT (`!`), and bitwise NOT (`~`). It ensures that these operations are performed correctly based on the type of the quantum value being operated upon.

## What it Does

The `execUnary` function takes a single quantum value `v` and an operation code `op`, and applies the specified unary operator to the value. The result of the operation is returned as a new quantum value. Supported operations include:

- **Negation (`Op::NEG`)**: Applies the negation operator to numeric values, reversing their sign.
- **Logical NOT (`Op::NOT`)**: Converts the quantum value to a boolean and then applies the logical NOT operator, flipping its truthiness.
- **Bitwise NOT (`Op::BIT_NOT`)**: Applies the bitwise NOT operator to numeric values, inverting each bit.

## Why it Works This Way

The function works by using a switch statement to handle different unary operations. Each case checks the type of the input quantum value `v` and performs the appropriate operation. For example, when handling negation, it checks if `v` is a number and then returns a new quantum value with the negated number. If `v` is not a number, it throws a `TypeError`. Similarly, for logical NOT, it checks if `v` is truthy or falsy and returns the opposite boolean value. Bitwise NOT also requires `v` to be a number and inverts each bit of the number.

This design allows for clear and concise handling of different unary operations, ensuring that each operation is applied correctly based on the type of the input value. By throwing errors for unsupported types, the function maintains robustness and prevents unexpected behavior during execution.

## Parameters/Return Value

### Parameters

- `v`: A `QuantumValue` object representing the quantum value to which the unary operator will be applied.
- `op`: An enumeration value of type `Op` indicating the unary operator to be executed (e.g., `Op::NEG`, `Op::NOT`, `Op::BIT_NOT`).

### Return Value

- Returns a new `QuantumValue` object containing the result of applying the unary operator to `v`.

## Edge Cases

1. **Unsupported Types**: If the input quantum value `v` is not of a supported type (e.g., attempting to apply negation to a non-numeric value), the function throws a `TypeError`.
2. **Truthiness Check**: When applying logical NOT, the function converts the quantum value to a boolean before performing the NOT operation. This means that any non-zero numeric value is considered true, and zero is considered false.
3. **Bitwise NOT**: The bitwise NOT operation is only defined for numeric values. Applying it to non-numeric values results in a `TypeError`.

## Interactions with Other Components

The `execUnary` function interacts with several other components within the Quantum Language compiler's virtual machine:

- **QuantumValue Class**: This class represents quantum values and provides methods for accessing their properties and performing operations like conversion to numbers or checking truthiness.
- **Error Handling**: The function uses custom error classes (`TypeError`, `RuntimeError`) to manage exceptions and provide meaningful error messages to the user.
- **Line Number Information**: The function accepts a `line` parameter, which indicates the line number where the operation was encountered. This information is used in error messages to help users locate the source of the issue.

Overall, the `execUnary` function plays a crucial role in executing unary operations within the Quantum Language compiler's virtual machine, ensuring that operations are performed correctly and providing informative error messages for unsupported types or invalid operations.