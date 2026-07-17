# `execUnary`

The `execUnary` function in the Quantum Language compiler's virtual machine core (`VmCore.cpp`) is designed to execute unary operations on quantum values. This function supports various unary operators including negation (`-`), logical NOT (`!`), and bitwise NOT (`~`). It ensures that the operation is performed correctly based on the type of the quantum value.

## What it Does

The `execUnary` function takes a single quantum value (`v`) and an operator (`op`) as input. Depending on the operator provided, it performs one of the following unary operations:

1. **Negation (`-`)**: If the quantum value is a number, it returns the negated value. For non-number types, it throws a `TypeError`.
2. **Logical NOT (`!`)**: Returns a boolean indicating whether the quantum value is falsy.
3. **Bitwise NOT (`~`)**: If the quantum value is a number, it performs a bitwise NOT operation and returns the result as a double. For non-number types, it throws a `TypeError`.

## Why it Works This Way

The function is structured in a way that allows for easy extension and maintenance. Each unary operator is handled in a separate `case` statement within a `switch` block. This design ensures that each operator has its own clear implementation and error handling path. The use of `throw` statements for errors provides a robust mechanism for signaling issues when unsupported operations or invalid data types are encountered.

## Parameters/Return Value

### Parameters

- `v`: A `QuantumValue` object representing the operand on which the unary operation will be applied.
- `op`: An `Op` enum value specifying the unary operator to be executed.

### Return Value

- Returns a `QuantumValue` object containing the result of the unary operation.
- Throws exceptions (`TypeError`, `RuntimeError`) in case of invalid operations or data types.

## Edge Cases

- **Non-number operands**: When applying negation (`-`) or bitwise NOT (`~`) to non-number quantum values, the function throws a `TypeError`. This ensures that only appropriate operations are performed on the given data type.
- **Falsiness check**: The logical NOT (`!`) operation checks for falsy conditions rather than just zero. This aligns with typical programming practices where any value considered false should trigger the NOT operation.

## Interactions with Other Components

The `execUnary` function interacts primarily with the `QuantumValue` class and the `Op` enumeration. It relies on these components to determine the type of the quantum value and to perform the necessary arithmetic or logical operations. Additionally, it uses exception handling mechanisms provided by the compiler's runtime environment to manage errors gracefully.

Overall, the `execUnary` function plays a crucial role in executing unary operations efficiently and safely within the Quantum Language compiler's virtual machine core, ensuring compatibility and correctness across different data types and operations.