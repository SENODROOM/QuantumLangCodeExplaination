# `execUnary` Function

## Purpose
The `execUnary` function is designed to execute unary operations on quantum values. Unary operations include negation (`-`), logical NOT (`!`), and bitwise NOT (`~`). This function ensures that only appropriate operations are performed based on the type of the quantum value.

## Parameters
- `op`: An enumeration representing the unary operation to be executed. Possible values are `Op::NEG`, `Op::NOT`, and `Op::BIT_NOT`.
- `v`: A `QuantumValue` object on which the unary operation will be applied.
- `line`: An integer representing the line number in the source code where the operation was encountered, used for error reporting.

## Return Value
The function returns a new `QuantumValue` object containing the result of the unary operation.

## Edge Cases
1. **Negation (`Op::NEG`)**:
   - If `v` is a number, the function negates the number.
   - If `v` is not a number, a `TypeError` is thrown indicating that negation cannot be applied to the given type.

2. **Logical NOT (`Op::NOT`)**:
   - The function checks if the quantum value is truthy or falsy using the `isTruthy()` method.
   - It then returns a `QuantumValue` object containing the boolean result of the NOT operation.

3. **Bitwise NOT (`Op::BIT_NOT`)**:
   - If `v` is a number, the function performs a bitwise NOT operation on the number and converts the result back to a double.
   - If `v` is not a number, a `TypeError` is thrown indicating that bitwise NOT cannot be applied to the given type.

## Interactions with Other Components
- **QuantumValue Class**: The function uses methods like `isNumber()`, `asNumber()`, and `isTruthy()` from the `QuantumValue` class to determine the type and value of the quantum value.
- **Error Handling**: The function throws exceptions (`TypeError` and `RuntimeError`) when encountering invalid types or unknown operations, interacting with the error handling mechanisms of the Quantum Language compiler.
- **Source Code Line Number**: The `line` parameter is passed to error messages to provide context about where the error occurred in the source code, facilitating debugging.

This function is crucial for handling unary operations within the Quantum Language compiler, ensuring type safety and providing meaningful error messages for unsupported operations.