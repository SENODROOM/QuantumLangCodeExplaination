# `execPrint` Function Explanation

The `execPrint` function in the Quantum Language compiler is designed to execute the `print` statement. It processes a `PrintStmt` object, which contains arguments that need to be printed and an optional separator (`sep`) and end character (`end`). The function evaluates each argument, formats them if necessary, and then outputs them to the standard console.

## Parameters
- **`s`**: A reference to a `PrintStmt` object. This object includes:
  - `args`: A vector of pointers to `Expr` objects representing the values to be printed.
  - `sep`: An optional string used to separate the printed arguments. If not provided, defaults to a space.
  - `end`: An optional string appended at the end of the print operation. If not provided, defaults to a newline character.

## Return Value
- **`void`**: The function does not return any value but directly prints to the standard output.

## How It Works
1. **Empty Arguments Check**:
   - If there are no arguments (`s.args.empty()`), the function simply prints the `end` character and flushes the output buffer.

2. **Evaluating Arguments**:
   - The function iterates over each expression in `s.args`, evaluates it using the `evaluate` function, and stores the results in a vector called `vals`.

3. **Formatting Mode Detection**:
   - The function checks if the first argument in `vals` is a string that contains a `%` format specifier (indicating Python's `printf` style formatting). If detected, it sets the `isPrintf` flag to `true`.

4. **Python-Style String Representation**:
   - A lambda function named `pyStr` is defined to provide a Python-like string representation for different types of `QuantumValue`. For example, booleans are represented as "True" or "False", and `nil` is represented as "None".

5. **Printing Arguments**:
   - If `isPrintf` is `true`, the function uses the `applyFormat` function to format the string based on the subsequent arguments in `vals`.
   - If `isPrintf` is `false`, the function iterates over each value in `vals`:
     - If the index `i` is greater than 0, it prints the separator (`s.sep`).
     - If the value is an instance, it attempts to find a method named `__str__` in the class hierarchy of the instance. If found, it calls this method and prints the result. If not found, it falls back to using the `pyStr` lambda function to get a string representation.
     - If the value is not an instance, it directly prints the value using the `pyStr` lambda function.

## Edge Cases
- **Empty Arguments**: When `s.args` is empty, the function prints only the `end` character.
- **No Separator**: If `s.sep` is not provided, the default separator is a space.
- **No End Character**: If `s.end` is not provided, the default end character is a newline.
- **Non-String Format Specifier**: If the first argument is a string but does not contain a `%` format specifier, the function treats it as normal print statements without formatting.
- **Missing `__str__` Method**: If an instance does not have a `__str__` method, the function falls back to using a Python-like string representation.

## Interactions with Other Components
- **Evaluator**: The `evaluate` function is used to evaluate expressions within `s.args`. This ensures that dynamic values are correctly computed before printing.
- **Formatter**: The `applyFormat` function is utilized when `isPrintf` is `true` to handle complex string formatting based on the `%` format specifiers.
- **Class Hierarchy**: The function interacts with the class hierarchy to find and invoke the `__str__` method for instances, allowing custom string representations of objects.

Overall, the `execPrint` function provides a flexible and robust mechanism for handling print operations in the Quantum Language compiler, supporting both simple and formatted output scenarios.