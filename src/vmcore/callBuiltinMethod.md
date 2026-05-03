# `callBuiltinMethod`

The `callBuiltinMethod` function is an essential part of the Quantum Language compiler's virtual machine (VM) core, responsible for handling the invocation of built-in methods on different data types including numbers, arrays, strings, dictionaries, and instances.

## What it Does

The `callBuiltinMethod` function checks the type of the object (`obj`) and then determines which built-in method should be invoked based on the provided `method` name and arguments (`args`). It supports various operations like formatting numbers, converting objects to strings, creating translation tables, and invoking asynchronous methods related to email management.

For example:
- If the object is a number and the method is `"toFixed"`, it converts the number to a string with a fixed number of decimal places specified by the first argument or defaults to zero if no argument is provided.
- If the object is a native instance and the method is `"maketrans"`, it creates a dictionary that can be used to translate characters from one set to another.
- For array, string, and dictionary types, it calls specialized helper functions (`callArrayMethod`, `callStringMethod`, `callDictMethod`) to handle the method invocation.

## Why it Works this Way

This design allows for a modular approach where each data type has its own handler for built-in methods. By separating the logic into different functions, it enhances readability and maintainability. Additionally, it provides a clear structure for adding new data types or methods without altering existing code.

## Parameters/Return Value

### Parameters
- `obj`: The object on which the built-in method is being called.
- `method`: A string representing the name of the built-in method to invoke.
- `args`: A vector of `QuantumValue` objects containing the arguments passed to the method.

### Return Value
- Returns a `QuantumValue` object representing the result of the method invocation.

## Edge Cases

- **Negative Precision**: When calling the `"toFixed"` method on a number with a negative precision, the function ensures that the precision is at least zero.
- **Empty Arguments**: If no arguments are provided when calling the `"toFixed"` method, it defaults to zero decimal places.
- **Non-existent Method**: If the method does not exist for the given object type, the function returns the original object unchanged.
- **Null or Invalid Native Instance**: If the native instance does not have a valid function pointer (`native->fn`), the function returns an empty `QuantumValue`.

## Interactions with Other Components

- **Object Type Checking**: The function interacts with the VM's type system to determine the type of the object (`isNumber`, `isNative`, etc.). This allows it to route the method invocation to the appropriate handler.
- **Helper Functions**: For complex data types like arrays, strings, and dictionaries, it uses helper functions (`callArrayMethod`, `callStringMethod`, `callDictMethod`) to perform the actual method invocation. These helper functions are designed to handle specific behaviors of each data type.
- **Email Management Methods**: For native instances representing email management, it invokes corresponding methods directly. This ensures that email-related operations are handled efficiently within the VM.

Overall, the `callBuiltinMethod` function plays a critical role in enabling built-in functionality across various data types, enhancing the expressiveness and usability of the Quantum Language compiler's VM.