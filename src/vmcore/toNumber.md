# toNumber

## Overview

The `toNumber` function is an essential utility within the Quantum Language compiler's Virtual Machine (VM) core. Its primary purpose is to convert a given value (`v`) into a numeric type, enabling subsequent arithmetic operations or comparisons on that value. This function checks if the input value is already a number and returns it directly. If the input value is a string, it attempts to convert the string to a double using `std::stod`. If the conversion fails, it throws a `TypeError`.

## Parameters

- `v`: The value to be converted to a number. It can be of any type supported by the Quantum Language compiler.
- `ctx`: A context string providing additional information about the error message, such as the operation being performed when the error occurs.
- `line`: An integer representing the line number where the error occurred, used for precise error reporting.

## Return Value

- Returns a `double` representation of the input value if successful.
- Throws a `TypeError` if the input value cannot be converted to a number.

## Edge Cases

1. **Already Number**: If the input value `v` is already a number, the function simply returns `v.asNumber()` without any further processing.
2. **Invalid String Conversion**: If the input value `v` is a string but cannot be converted to a valid double using `std::stod`, the function throws a `TypeError`.
3. **Empty String**: Converting an empty string to a number will result in a `TypeError` because there is nothing to parse.
4. **Non-Numeric Strings**: Strings containing non-numeric characters (e.g., "abc") will also result in a `TypeError`.

## Interactions with Other Components

The `toNumber` function interacts primarily with the VM core and its associated data types. It utilizes methods like `isNumber()`, `asNumber()`, and `typeName()` from the `Value` class to determine the type of the input value and retrieve its numeric representation or type name, respectively. When converting a string to a number, it leverages the standard library function `std::stod` to perform the actual conversion. If the conversion fails, it catches the exception and throws a custom `TypeError` to inform the user about the incorrect type encountered during the operation.