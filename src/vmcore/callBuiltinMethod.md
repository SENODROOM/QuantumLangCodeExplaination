# `callBuiltinMethod`

The `callBuiltinMethod` function is an integral component of the Quantum Language compiler's virtual machine (VM) core, designed to manage the execution of built-in methods across various data types such as numbers, arrays, strings, dictionaries, and instances.

## What it Does

The primary role of `callBuiltinMethod` is to invoke predefined built-in methods associated with different data types. This function ensures that when a built-in method is called on an object, the appropriate implementation is executed, thereby providing functionality like formatting numbers, converting objects to strings, or performing operations on collections.

## Why It Works This Way

This design allows `callBuiltinMethod` to be flexible and extensible. By checking the type of the object (`isNumber`, `isNative`, etc.), the function can determine which set of built-in methods to utilize. For instance, numeric objects have methods like `toFixed` and `toString`, while native objects might have methods like `maketrans`. The use of conditional checks ensures that only relevant methods are invoked, optimizing performance and preventing errors.

## Parameters/Return Value

- **Parameters**:
  - `obj`: A `QuantumValue` representing the object on which the built-in method is being called.
  - `method`: A `std::string` indicating the name of the built-in method to be invoked.
  - `args`: A `std::vector<QuantumValue>` containing any arguments required by the method.

- **Return Value**:
  - Returns a `QuantumValue` representing the result of the method invocation. If the method does not exist or cannot be invoked, it returns an empty `QuantumValue`.

## Edge Cases

- **Negative Precision**: When calling the `toFixed` method on a number with a negative precision, `callBuiltinMethod` ensures that the precision is set to 0, avoiding invalid formatting.
- **Empty Arguments**: If no arguments are provided to the `toFixed` method, it defaults to formatting the number without any decimal places.
- **Non-existent Method**: If the specified method does not exist for the given object type, `callBuiltinMethod` handles this gracefully by returning an empty `QuantumValue`.
- **Null Object**: Although not explicitly handled in the provided code snippet, `callBuiltinMethod` should ideally check for null objects to prevent runtime errors.

## Interactions with Other Components

- **Object Type Checking**: `callBuiltinMethod` interacts with the VM's type system to determine the type of the object (`isNumber`, `isNative`, etc.). This interaction is crucial for identifying the correct set of built-in methods.
- **Method Invocation**: Depending on the object type, `callBuiltinMethod` calls specialized functions (`callArrayMethod`, `callStringMethod`, etc.) to handle method invocations. These specialized functions further interact with the respective data structures to perform the requested operations.
- **Error Handling**: Throughout its execution, `callBuiltinMethod` includes error handling mechanisms to manage edge cases and ensure robustness. This interaction with error handling components helps maintain the stability of the VM.

In summary, `callBuiltinMethod` serves as a central hub for invoking built-in methods on various data types within the Quantum Language compiler's VM core. Its modular approach and comprehensive error handling make it a vital part of ensuring the compiler's functionality and reliability.