# `execUnary`

The `execUnary` function in the Quantum Language compiler's virtual machine core (`VmCore.cpp`) is designed to execute unary operations on quantum values. This function supports various unary operators including negation (`-`), logical NOT (`!`), and bitwise NOT (`~`). It ensures that the operation is performed correctly based on the type of the quantum value and throws appropriate errors for unsupported types or unknown operators.

## Functionality

The `execUnary` function takes two parameters:
1. `op`: An enumeration representing the unary operator to be executed.
2. `v`: A `QuantumValue` object representing the operand on which the unary operation will be applied.

The function returns a new `QuantumValue` object containing the result of the unary operation.

### Supported Unary Operators

- **Negation (`Op::NEG`)**: Applies the negation operator to the numeric value of the quantum variable. If the quantum value is not a number, it throws a `TypeError`.
  
- **Logical NOT (`Op::NOT`)**: Applies the logical NOT operator to the truthiness of the quantum value. The function checks if the quantum value is truthy (i.e., non-zero, non-empty, etc.) and returns a boolean value indicating whether the original value was falsy.

- **Bitwise NOT (`Op::BIT_NOT`)**: Applies the bitwise NOT operator to the numeric value of the quantum variable. Similar to negation, if the quantum value is not a number, it throws a `TypeError`.

### Edge Cases

- **Non-Numeric Operand**: When applying negation or bitwise NOT, if the quantum value is not a number, the function throws a `TypeError`. For example, attempting to negate a quantum string or array would result in an error.
  
- **Unknown Operator**: If the provided unary operator is not recognized, the function throws a `RuntimeError`.

## Implementation Details

Here’s how the `execUnary` function implements its functionality:

```cpp
QuantumValue VmCore::execUnary(Op op, const QuantumValue& v, int line) {
    switch (op) {
    case Op::NEG:
        // Check if the quantum value is a number
        if (v.isNumber()) 
            // Return the negated number
            return QuantumValue(-v.asNumber());
        else 
            // Throw an error if the value is not a number
            throw TypeError("Unary - on " + v.typeName(), line);
    case Op::NOT:
        // Return the logical NOT of the quantum value
        return QuantumValue(!v.isTruthy());
    case Op::BIT_NOT:
        // Check if the quantum value is a number
        if (v.isNumber()) 
            // Apply bitwise NOT and cast back to double
            return QuantumValue((double)(~(long long)v.asNumber()));
        else 
            // Throw an error if the value is not a number
            throw TypeError("Bitwise ~ on " + v.typeName(), line);
    default:
        // Throw an error for unknown unary operators
        throw RuntimeError("Unknown unary op", line);
    }
}
```

### Explanation of Each Case

- **Negation (`Op::NEG`)**:
  ```cpp
  if (v.isNumber()) 
      return QuantumValue(-v.asNumber());
  else 
      throw TypeError("Unary - on " + v.typeName(), line);
  ```
  - Checks if the quantum value is a number using `v.isNumber()`.
  - If true, negates the number using `-v.asNumber()` and returns the result wrapped in a `QuantumValue`.
  - If false, throws a `TypeError` specifying that the unary minus cannot be applied to the given type.

- **Logical NOT (`Op::NOT`)**:
  ```cpp
  return QuantumValue(!v.isTruthy());
  ```
  - Returns a new `QuantumValue` containing the logical NOT of the quantum value. This is determined by checking if the quantum value is truthy using `v.isTruthy()`.

- **Bitwise NOT (`Op::BIT_NOT`)**:
  ```cpp
  if (v.isNumber()) 
      return QuantumValue((double)(~(long long)v.asNumber()));
  else 
      throw TypeError("Bitwise ~ on " + v.typeName(), line);
  ```
  - Checks if the quantum value is a number using `v.isNumber()`.
  - If true, applies the bitwise NOT using `~(long long)v.asNumber()` and casts the result back to a `double`, returning it wrapped in a `QuantumValue`.
  - If false, throws a `TypeError` specifying that the bitwise NOT cannot be applied to the given type.

### Interactions with Other Components

The `execUnary` function interacts with several other components within the Quantum Language compiler's virtual machine core:
- **Type Checking**: Uses methods like `isNumber()` and `isTruthy()` to ensure that the quantum value is of the correct type before performing the unary operation.
- **Error Handling**: Throws exceptions like `TypeError` and `RuntimeError` when encountering invalid input or unknown operators, ensuring robust error handling throughout the execution process.

Overall, the `execUnary` function plays a crucial role in processing unary operations efficiently and safely within the Quantum Language compiler's virtual machine core.