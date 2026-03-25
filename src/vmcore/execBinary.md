# `execBinary`

The `execBinary` function is a crucial method within the Quantum Language compiler's virtual machine core (`VmCore.cpp`). This function is responsible for executing binary operations between two quantum values (`L` and `R`) based on the specified operation type (`op`). It handles various data types including strings, arrays, numbers, and booleans, ensuring that operations can be performed across different types seamlessly.

## Parameters

- **`Op op`**: The type of binary operation to perform. This parameter is an enumeration that defines the supported operations such as addition (`Op::ADD`), equality check (`Op::EQ`), inequality check (`Op::NEQ`), multiplication (`Op::MUL`), division (`Op::DIV`), modulo (`Op::MOD`), and floor division (`Op::FLOOR_DIV`).
  
- **`QuantumValue L`**: The left-hand side operand of the binary operation. This is a quantum value object which can represent strings, arrays, numbers, or booleans.

- **`QuantumValue R`**: The right-hand side operand of the binary operation. Similar to `L`, this is also a quantum value object.

- **`int line`**: The line number in the source code where the binary operation occurs. This parameter is used to provide context in error messages related to the operation.

## Return Value

The function returns a `QuantumValue` object representing the result of the binary operation. Depending on the operation type and the data types of the operands, the result could be a string, array, number, boolean, etc.

## Edge Cases

1. **String Addition**: If either `L` or `R` is a string, the function performs string concatenation instead of numeric addition. For example, `"hello" + "world"` results in `"helloworld"`.

2. **Array Addition**: If both `L` and `R` are arrays, the function concatenates them into a new array. For example, `[1, 2] + [3, 4]` results in `[1, 2, 3, 4]`.

3. **Numeric Operations**: If both `L` and `R` are numeric types (number, string representation of a number, or boolean), the function converts them to numbers and then performs the specified arithmetic operation. For example, `5 + "3"` results in `8`.

4. **Equality and Inequality Checks**: The function allows for mixed types when checking equality (`Op::EQ`) and inequality (`Op::NEQ`). It uses a helper function `valuesEqual` to compare the values, converting non-numeric types to numbers before comparison.

5. **Division and Modulo by Zero**: When performing division (`Op::DIV`) or modulo (`Op::MOD`) operations, the function checks if the divisor is zero. If so, it throws a `RuntimeError` indicating division or modulo by zero at the given line number.

6. **Floor Division**: For floor division (`Op::FLOOR_DIV`), similar to regular division, the function checks if the divisor is zero. If not, it performs floor division using `std::floor` on the result of the division.

## Interactions with Other Components

- **`QuantumValue` Class**: The function extensively uses the `QuantumValue` class to handle different data types and their conversions. This class likely provides methods like `isString()`, `toString()`, `isArray()`, `asArray()`, `isNumber()`, `asNumber()`, `isBool()`, and `asBool()` to interact with quantum values.

- **Helper Functions**: The function calls a helper function `valuesEqual` for equality checks. This function might be defined elsewhere in the compiler's codebase and is responsible for comparing quantum values, taking into account type conversions.

- **Error Handling**: The function includes error handling mechanisms to manage exceptions, particularly when dealing with division and modulo operations by zero. These errors are propagated up through the call stack as `RuntimeError` objects, providing detailed information about the location and nature of the error.

Overall, the `execBinary` function plays a vital role in the execution phase of the Quantum Language compiler by handling binary operations across different data types efficiently and robustly. Its design ensures that operations are performed correctly while gracefully managing potential errors.