# `toString` Function

## Overview

The `toString` function is a member method of the `Token` class within the Quantum Language compiler's source code located at `src/Token.cpp`. This function serves to convert a `Token` object into a human-readable string format, which includes essential details like the token's type, line number, and value.

### Parameters/Return Value

- **Parameters**: None.
- **Return Value**: A `std::string` representing the stringified version of the `Token`.

### Edge Cases

1. **Empty Token Values**: If the `value` attribute of the `Token` is an empty string, the function will still correctly format the output as `[line:col []]`.
2. **Special Characters in Value**: The function handles special characters gracefully by inserting them directly into the string without any additional escaping or formatting.

### Interactions with Other Components

The `toString` function interacts with the following components:

- **Line Number (`line`)**: It retrieves the line number where the token was encountered during parsing.
- **Column Number (`col`)**: It fetches the column number within that line where the token starts.
- **Token Value (`value`)**: It accesses the actual value of the token, which could be a keyword, identifier, literal, etc., depending on the token type.

These values are then formatted using `std::ostringstream`, which constructs a string stream to hold the formatted output. The formatted string is returned by the function.

### Why It Works This Way

This implementation ensures that each token can be easily identified and debugged by providing context about its location in the source code and its content. By encapsulating these details within a single string, the `toString` function facilitates logging, error reporting, and general debugging processes in the compiler. The use of `std::ostringstream` allows for efficient string construction and manipulation, making the function both readable and performant.