# toNumber

## Overview

The `toNumber` function is a crucial utility within the Quantum Language compiler's Virtual Machine (VM) core. Its main objective is to transform a provided value (`v`) into a numeric data type, facilitating arithmetic operations and comparisons. The function operates by first checking if the input value is already of a numeric type. If so, it simply returns the numeric representation. If not, it attempts to convert the string representation of the value into a double using `std::stod`. If the conversion fails due to invalid characters or format, the function throws a `TypeError`.

## Parameters

- `v`: The input value that needs to be converted into a numeric type. It can be either a numeric type or a string representing a numeric value.
- `ctx`: A context string indicating where the error occurred, aiding in debugging.
- `line`: The line number at which the error occurred, also assisting in debugging.

## Return Value

- Returns a `double` representing the numeric value of the input.

## Edge Cases

1. **Numeric Input**: If the input value is already a numeric type, the function directly returns its numeric representation without any conversion.
2. **String Input**: If the input value is a string, the function tries to convert it into a double. If the string contains non-numeric characters or is formatted incorrectly, the conversion will fail, and the function will throw a `TypeError`.
3. **Invalid Input Type**: If the input value is neither a numeric type nor a string, the function throws a `TypeError`.

## Interactions with Other Components

The `toNumber` function interacts with various components within the VM core:

1. **Value Class**: It utilizes methods from the `Value` class to check if the input value is a number or a string and to retrieve their respective representations.
2. **Error Handling**: It leverages the `TypeError` exception to handle errors related to invalid input types or formatting issues during the conversion process.

This utility ensures that values are consistently treated as numbers throughout the compilation and execution phases, thereby simplifying arithmetic operations and comparisons.