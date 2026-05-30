# `execUnary`

The `execUnary` function in the Quantum Language compiler's virtual machine core (`VmCore.cpp`) is designed to execute unary operations on quantum values. This function supports various unary operators including negation (`-`), logical NOT (`!`), and bitwise NOT (`~`). It ensures that the operation is performed correctly based on the type of the quantum value.

## What it does

The `execUnary` function takes a single quantum value `v` and an operator `op`, and applies the specified unary operation to `v`. The result of the operation is returned as a new quantum value.

## Why it works this way

The function uses a `switch` statement to handle different unary operators. For each operator, it checks the type of the input quantum value `v` and performs the appropriate operation:

1. **Negation (`Op::NEG`)**: If `v` is a number, it returns the negated value. Otherwise, it throws a `TypeError`.
2. **Logical NOT (`Op::NOT`)**: It returns a boolean value indicating whether `v` is falsy or truthy.
3. **Bitwise NOT (`Op::BIT_NOT`)**: If `v` is a number, it performs a bitwise NOT operation on its integer representation and returns the result as a double. Otherwise, it throws a `TypeError`.

This approach allows the function to handle different types of quantum values and apply the correct unary operation, ensuring type safety and consistency.

## Parameters/Return Value

### Parameters

- `v`: A `QuantumValue` object representing the input quantum value.
- `op`: An `Op` enum value representing the unary operator to be applied.

### Return Value

- Returns a `QuantumValue` object representing the result of applying the unary operator to `v`.

## Edge Cases

- When applying the negation operator (`Op::NEG`), if `v` is not a number, a `TypeError` is thrown.
- When applying the bitwise NOT operator (`Op::BIT_NOT`), if `v` is not a number, a `TypeError` is thrown.
- When applying the logical NOT operator (`Op::NOT`), if `v` is not a boolean-like value, the function may still return a valid result based on its internal logic.

## Interactions with Other Components

The `execUnary` function interacts with the `QuantumValue` class to perform type-specific operations. It also interacts with the `RuntimeError` and `TypeError` classes to handle errors related to unsupported operators or invalid operand types. Additionally, it may interact with other parts of the virtual machine core to manage the state of quantum registers and values during execution.