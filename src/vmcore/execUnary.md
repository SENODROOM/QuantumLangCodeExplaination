# `execUnary`

The `execUnary` function is designed to execute unary operations on quantum values within the Quantum Language compiler's virtual machine core (`VmCore.cpp`). It processes various unary operators including negation (`-`), logical NOT (`!`), and bitwise NOT (`~`), applying them to the provided quantum value.

## Parameters
- `op`: An enumeration representing the unary operator to be executed.
- `v`: A `QuantumValue` object on which the unary operation will be applied.
- `line`: An integer indicating the line number where the error occurred, used for debugging purposes.

## Return Value
- Returns a new `QuantumValue` object after applying the unary operation.

## Edge Cases
1. **Negation (`Op::NEG`)**:
   - If the input quantum value `v` is a number, the function returns a new `QuantumValue` with the negated numeric value.
   - If `v` is not a number, it throws a `TypeError`.

2. **Logical NOT (`Op::NOT`)**:
   - The function returns a new `QuantumValue` with the result of the logical NOT operation. It checks whether the quantum value is truthy or falsy using the `isTruthy()` method.

3. **Bitwise NOT (`Op::BIT_NOT`)**:
   - If the input quantum value `v` is a number, the function performs a bitwise NOT operation on its binary representation and returns a new `QuantumValue` with the resulting numeric value.
   - If `v` is not a number, it throws a `TypeError`.

## How It Works
The `execUnary` function uses a `switch` statement to determine the type of unary operation specified by the `op` parameter. Depending on the operator, it either negates the numeric value, performs a logical NOT check, or executes a bitwise NOT operation. Each case handles the specific type of quantum value appropriately, ensuring that only valid operations are performed and that errors are handled gracefully.

### Detailed Execution Flow
1. **Negation (`Op::NEG`)**:
   - Checks if the quantum value `v` is a number using `v.isNumber()`.
   - If true, negates the numeric value using `-v.asNumber()`.
   - Returns a new `QuantumValue` with the negated value.
   - If false, throws a `TypeError` indicating that negation cannot be applied to non-number types.

2. **Logical NOT (`Op::NOT`)**:
   - Uses the `isTruthy()` method to evaluate whether the quantum value `v` is considered truthy.
   - Returns a new `QuantumValue` with the boolean result of the logical NOT operation (`!v.isTruthy()`).

3. **Bitwise NOT (`Op::BIT_NOT`)**:
   - Checks if the quantum value `v` is a number using `v.isNumber()`.
   - If true, converts the numeric value to a `long long` and performs a bitwise NOT operation using `~(long long)v.asNumber()`.
   - Converts the result back to a `double` and returns a new `QuantumValue` with the bitwise NOT result.
   - If false, throws a `TypeError` indicating that bitwise NOT cannot be applied to non-number types.

4. **Default Case**:
   - If an unknown unary operator is encountered, the function throws a `RuntimeError` indicating an unknown unary operation.

## Interactions With Other Components
- **QuantumValue Class**: The function relies on methods like `isNumber()`, `asNumber()`, and `isTruthy()` from the `QuantumValue` class to perform the necessary checks and conversions.
- **Error Handling**: The function interacts with error handling mechanisms provided by the compiler, throwing `TypeError` and `RuntimeError` as appropriate when encountering invalid operations or unknown operators.
- **Virtual Machine Core**: The `execUnary` function is part of the broader functionality of the virtual machine core, responsible for executing various quantum operations efficiently and accurately.

By understanding how `execUnary` processes different unary operators and interacts with other components, developers can better utilize and extend the capabilities of the Quantum Language compiler's virtual machine core.