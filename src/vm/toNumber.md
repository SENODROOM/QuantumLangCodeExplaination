# `toNumber` Function

## Purpose
The `toNumber` function is designed to convert a given value (`v`) into a numerical type. This conversion is essential for operations that require numeric input, such as arithmetic calculations or comparisons.

## Parameters
- `v`: The value to be converted. It can be of any type supported by the Quantum Language compiler, including numbers and strings.
- `ctx`: A string representing the context in which the conversion is being attempted. This parameter helps provide more informative error messages.
- `line`: An integer indicating the line number where the conversion is being performed. This aids in debugging by pinpointing the exact location of an issue.

## Return Value
The function returns a `double`, which represents the numerical equivalent of the input value (`v`). If the input value cannot be converted to a number, the function throws a `TypeError`.

## How It Works
1. **Check if `v` is already a Number**: The function first checks if the input value (`v`) is already a number using the `isNumber()` method. If true, it directly returns the number using the `asNumber()` method without any further processing.

2. **Convert String to Number**: If the input value is not a number but is a string, the function attempts to convert the string to a double using `std::stod()`. This method parses the string and converts it to a floating-point number. If the string contains non-numeric characters or is formatted incorrectly, `std::stod()` will throw an exception.

3. **Error Handling**: In case of an exception thrown during the string-to-number conversion, the function catches it silently (using `catch (...)`) and proceeds to the next step.

4. **Throw TypeError**: If the input value is neither a number nor a convertible string, the function throws a `TypeError`. This exception includes a message that specifies the expected type (`number`) and the actual type encountered (`v.typeName()`), along with the line number where the error occurred.

## Edge Cases
- **Empty String**: If the input string is empty, `std::stod()` will throw an exception because there is nothing to parse.
- **Non-Numeric Characters**: Any string containing non-numeric characters will fail to convert to a number and result in a `TypeError`.
- **Invalid Format**: Strings formatted in ways that do not conform to standard numeric representations (e.g., missing decimal points) will also cause a `TypeError`.

## Interactions with Other Components
The `toNumber` function interacts primarily with the value object (`v`) and the error handling mechanism provided by the Quantum Language compiler. It leverages the `isNumber()` and `asNumber()` methods of the value object to check and retrieve its numeric representation. When converting a string to a number, it utilizes the standard library's `std::stod()` function, which is part of the `<string>` header. The function also relies on the `typeName()` method of the value object to include the actual type in the error message when throwing a `TypeError`.

In summary, the `toNumber` function serves as a versatile tool for ensuring that values are in a numerical format, facilitating operations that depend on numeric data. Its robust error handling ensures that the compiler can gracefully manage unexpected inputs, improving overall reliability and user experience.