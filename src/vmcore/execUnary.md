# `execUnary`

The `execUnary` function is designed to execute unary operations on quantum values in the Quantum Language compiler's virtual machine core (`VmCore.cpp`). This function processes different unary operators such as negation (`-`), logical NOT (`!`), and bitwise NOT (`~`) and applies them to the provided quantum value (`v`). The result of the operation is returned as a new `QuantumValue`.

## Parameters

- `op`: An enumeration representing the unary operator to be executed. Possible values include:
  - `Op::NEG`: Represents the negation operator `-`.
  - `Op::NOT`: Represents the logical NOT operator `!`.
  - `Op::BIT_NOT`: Represents the bitwise NOT operator `~`.
- `v`: A `QuantumValue` object on which the unary operation will be performed.
- `line`: An integer representing the line number in the source code where the operation occurs, used for error reporting.

## Return Value

The function returns a `QuantumValue` that represents the result of applying the specified unary operator to the input `v`. The type of the returned value depends on the operator being applied:

- For `Op::NEG`, if `v` is a number, the function returns its negated value; otherwise, it throws an exception.
- For `Op::NOT`, the function returns a boolean value indicating whether `v` is falsy or truthy.
- For `Op::BIT_NOT`, if `v` is a number, the function performs a bitwise NOT operation on it and returns the result as a double; otherwise, it throws an exception.

## Edge Cases

1. **Negation (`Op::NEG`)**:
   - If `v` is not a number, the function throws a `TypeError` with a message indicating the unsupported operation on the given type.
   
2. **Logical NOT (`Op::NOT`)**:
   - The function checks if `v` is falsy using the `isTruthy()` method. If `v` is falsy, it returns `true`; otherwise, it returns `false`.

3. **Bitwise NOT (`Op::BIT_NOT`)**:
   - Similar to negation, if `v` is not a number, the function throws a `TypeError` with a message indicating the unsupported operation on the given type.

## Interactions with Other Components

- **Error Handling**: The function interacts with the error handling mechanisms of the compiler. It uses exceptions (`throw`) to report errors when an unsupported unary operation is attempted on a non-number quantum value.
  
- **Type Checking**: The function performs type checking on the input `v` to ensure that the unary operation can be safely applied. This interaction with type information is crucial for maintaining the integrity of the quantum computation process.

- **Virtual Machine Core**: As part of the `VmCore.cpp` file, `execUnary` is integrated into the broader context of the virtual machine core responsible for executing quantum programs. It leverages the capabilities of the `QuantumValue` class to handle various data types and operations within the quantum language environment.

In summary, the `execUnary` function is essential for performing unary operations on quantum values in the Quantum Language compiler. Its design ensures robustness through thorough type checking and appropriate error handling, making it a vital component of the virtual machine core.