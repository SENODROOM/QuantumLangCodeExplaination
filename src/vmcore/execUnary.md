# `execUnary`

The `execUnary` function in the Quantum Language compiler's virtual machine core (`VmCore.cpp`) is designed to execute unary operations on quantum values. This function supports various unary operators including negation (`-`), logical NOT (`!`), and bitwise NOT (`~`). It ensures that the operation is performed correctly based on the type of the quantum value provided.

## What it Does

The `execUnary` function takes a single parameter, a quantum value (`QuantumValue v`), and applies a specified unary operator (`Op op`) to it. The result of the operation is returned as a new quantum value.

## Parameters/Return Value

### Parameters
- `op`: An enumeration representing the unary operator to be applied. Supported operators include `Op::NEG` (negation), `Op::NOT` (logical NOT), and `Op::BIT_NOT` (bitwise NOT).
- `v`: A quantum value on which the unary operation will be executed.

### Return Value
- Returns a new quantum value representing the result of applying the unary operator to the input quantum value.

## Edge Cases

1. **Negation (`Op::NEG`)**:
   - If the quantum value is a number, the function returns its negation.
   - If the quantum value is not a number, an exception (`TypeError`) is thrown indicating that the negation operation cannot be performed on the given type.

2. **Logical NOT (`Op::NOT`)**:
   - The function checks if the quantum value is truthy using the `isTruthy()` method.
   - It returns a new quantum value with the boolean negation of the truthiness result.

3. **Bitwise NOT (`Op::BIT_NOT`)**:
   - If the quantum value is a number, the function performs a bitwise NOT operation on its integer representation and returns the result as a double.
   - If the quantum value is not a number, an exception (`TypeError`) is thrown indicating that the bitwise NOT operation cannot be performed on the given type.

4. **Unknown Operator**:
   - If the operator passed to the function is not recognized (`default` case), an exception (`RuntimeError`) is thrown indicating that the unary operation is unknown.

## Interactions with Other Components

The `execUnary` function interacts with several other components within the Quantum Language compiler:

- **QuantumValue Class**: This class represents quantum values and provides methods like `asNumber()`, `isNumber()`, and `isTruthy()` to manipulate and check the type of the quantum value.
- **Exception Handling**: The function uses custom exceptions (`TypeError` and `RuntimeError`) to handle errors related to unsupported operations or invalid types.
- **Line Information**: The function accepts a `line` parameter to provide context information when throwing exceptions, helping in debugging and error reporting.

Overall, the `execUnary` function plays a crucial role in handling unary operations within the Quantum Language compiler's virtual machine, ensuring type safety and correct execution of operations.