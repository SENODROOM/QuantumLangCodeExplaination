# `execUnary`

The `execUnary` function in the Quantum Language compiler's virtual machine core (`VmCore.cpp`) is designed to execute unary operations on quantum values. This function supports various unary operators including negation (`-`), logical NOT (`!`), and bitwise NOT (`~`). It ensures that the operation is performed correctly based on the type of the quantum value and throws appropriate errors for unsupported types or unknown operators.

## What it Does

The `execUnary` function takes an operator (`Op`) and a quantum value (`v`) as input and returns the result of applying the unary operator to the quantum value. The supported unary operators are:

1. **Negation (`Op::NEG`)**: Applies the negation operator to numeric quantum values. If the quantum value is not numeric, it throws a `TypeError`.
2. **Logical NOT (`Op::NOT`)**: Applies the logical NOT operator to quantum values. It checks if the quantum value is truthy and returns the opposite boolean value.
3. **Bitwise NOT (`Op::BIT_NOT`)**: Applies the bitwise NOT operator to numeric quantum values. If the quantum value is not numeric, it throws a `TypeError`.

## Why it Works This Way

The function works this way because it needs to handle different types of quantum values and apply the appropriate unary operator. For negation and bitwise NOT, it specifically checks if the quantum value is numeric. For logical NOT, it checks if the quantum value is truthy. If an unsupported type or operator is encountered, it throws an error to prevent incorrect behavior or crashes.

## Parameters/Return Value

### Parameters

- `op`: An enumeration representing the unary operator to be applied. Supported values are `Op::NEG`, `Op::NOT`, and `Op::BIT_NOT`.
- `v`: A `QuantumValue` object representing the quantum value on which the unary operator will be applied.
- `line`: An integer representing the line number where the unary operation was encountered in the source code. Used for error reporting.

### Return Value

- Returns a `QuantumValue` object containing the result of the unary operation.

## Edge Cases

1. **Non-Numeric Values**: When applying negation or bitwise NOT to non-numeric quantum values, the function throws a `TypeError`. For example, attempting to negate a string or a quantum register results in an error.
2. **Truthiness Check**: The logical NOT operator uses a truthiness check to determine the boolean value. In Quantum Language, certain values like zero, empty strings, and false registers are considered falsy, while others are considered truthy.

## Interactions with Other Components

The `execUnary` function interacts with several other components within the Quantum Language compiler's virtual machine:

1. **QuantumValue Class**: This class represents quantum values and provides methods to access their properties (e.g., whether they are numbers or truthy).
2. **Error Handling Mechanisms**: The function uses custom exceptions (`TypeError` and `RuntimeError`) to report errors when encountering unsupported types or unknown operators.
3. **Source Code Line Number Tracking**: By passing the line number (`line`) to the function, it can provide more accurate error messages, helping developers locate issues in their source code.

This comprehensive approach ensures that the `execUnary` function handles unary operations robustly and provides clear feedback for any errors encountered during execution.