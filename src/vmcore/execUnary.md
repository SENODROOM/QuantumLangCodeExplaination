# `execUnary`

The `execUnary` function in the Quantum Language compiler's virtual machine core (`VmCore.cpp`) is responsible for executing unary operations on quantum values. This function handles different unary operators such as negation (`-`), logical NOT (`!`), and bitwise NOT (`~`).

## What it Does

The primary role of `execUnary` is to apply a specified unary operator to a given quantum value and produce a new quantum value that reflects the result of the operation. The function checks the type of the quantum value and applies the appropriate operation based on the operator code provided.

## Why it Works This Way

The function uses a `switch` statement to determine which unary operator to apply. Each case in the `switch` corresponds to a specific operator:

1. **Op::NEG**: For negation, the function checks if the quantum value is a number. If it is, the function negates the number using the `-` operator and returns a new quantum value with the negated number. If the quantum value is not a number, an exception of type `TypeError` is thrown, indicating that the negation operation cannot be performed on the given type.

2. **Op::NOT**: For logical NOT, the function checks if the quantum value is truthy or falsy. It then returns a new quantum value with the opposite boolean state.

3. **Op::BIT_NOT**: For bitwise NOT, the function also checks if the quantum value is a number. If it is, the function performs a bitwise NOT operation using the `~` operator. The result is cast back to a double before returning a new quantum value. If the quantum value is not a number, an exception of type `TypeError` is thrown.

The use of a `switch` statement allows for clear and concise handling of each unary operator, making the code easy to read and maintain.

## Parameters/Return Value

### Parameters

- `op`: An enumeration value representing the unary operator to be executed. Possible values include `Op::NEG`, `Op::NOT`, and `Op::BIT_NOT`.
- `v`: A `QuantumValue` object representing the quantum value on which the unary operation will be applied.
- `line`: An integer representing the line number where the unary operation was encountered in the source code.

### Return Value

The function returns a new `QuantumValue` object containing the result of the unary operation.

## Edge Cases

1. **Non-numeric values for negation and bitwise NOT**: When applying negation or bitwise NOT to non-numeric quantum values, the function throws a `TypeError`. This ensures that only valid types for these operations are processed.

2. **Empty or null quantum values**: Although not explicitly mentioned in the provided code, typical implementations would handle empty or null quantum values gracefully, possibly by throwing an exception or returning a default value.

## Interactions with Other Components

The `execUnary` function interacts closely with the `QuantumValue` class, which likely contains methods for checking the type of the quantum value (`isNumber()`, `isTruthy()`), retrieving its value (`asNumber()`), and creating new quantum values (`QuantumValue(double)`). Additionally, it may interact with error handling mechanisms to manage exceptions like `TypeError` and `RuntimeError`.

Overall, the `execUnary` function plays a crucial role in performing unary operations within the Quantum Language compiler's virtual machine, ensuring that operations are correctly handled based on the type of the quantum value involved.