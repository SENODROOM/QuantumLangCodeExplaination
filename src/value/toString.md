# `toString` Method Explanation

## Overview

The `toString` method in the Quantum Language compiler converts various quantum data types into their corresponding string representations. This functionality is crucial for debugging, logging, and user interface interactions where textual output is required.

## Parameters

- None

## Return Value

- Returns a `std::string` that represents the object's value as a string.

## Edge Cases

- **QuantumNil**: The method returns `"nil"` when encountering a `QuantumNil` object.
- **bool**: For boolean values, the method returns either `"true"` or `"false"`.
- **double**: If the double value is an integer and its absolute value is less than \(1 \times 10^{15}\), it returns the value as a `long long`. Otherwise, it formats the double to a string with 10 decimal places.
- **std::string**: Directly returns the string value.
- **std::shared_ptr<Array>**: Converts an array to a string representation. Each element of the array is converted to a string, and elements are separated by commas. Strings within the array are enclosed in quotes.
- **std::shared_ptr<Dict>**: Converts a dictionary to a string representation. Each key-value pair is represented as `"key": value`, and pairs are separated by commas. Strings within the dictionary are enclosed in quotes.
- **std::shared_ptr<Closure>**: Returns a string indicating that the object is a closure, along with its name.
- **std::shared_ptr<QuantumNative>**: Returns a string indicating that the object is a native function, along with its name.
- **std::shared_ptr<QuantumInstance>**: Calls the `__str__` method if it is defined for the instance's class. If not, it falls back to converting the instance to a string representation based on its class name and properties.

## Interactions with Other Components

- **Debugging**: The `toString` method is used extensively for printing the state of objects during debugging sessions.
- **Logging**: When logging events or errors, the `toString` method helps in generating human-readable logs.
- **User Interface**: In interactive environments like the Quantum Shell, the `toString` method is used to display the results of operations in a readable format.

This method ensures that all quantum data types can be easily converted to strings, making it a versatile tool for various parts of the compiler and runtime system.