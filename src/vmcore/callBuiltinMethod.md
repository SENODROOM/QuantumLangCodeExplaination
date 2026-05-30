# `callBuiltinMethod`

The `callBuiltinMethod` function is a crucial part of the Quantum Language compiler's virtual machine (VM) core, designed to execute built-in methods on various data types including numbers, arrays, strings, dictionaries, and instances. This function enables dynamic method invocation based on the type of the object and the specified method name, ensuring that each data type can utilize its inherent functionalities effectively.

## What It Does

The `callBuiltinMethod` function takes three parameters:
- `obj`: The object on which the method is to be called.
- `method`: A string representing the name of the method to invoke.
- `args`: A vector of `QuantumValue` objects containing the arguments to pass to the method.

It returns a `QuantumValue` object, which represents the result of the method call. If the method is not found or cannot be executed, it may return an empty `QuantumValue`.

## Why It Works This Way

This implementation allows for polymorphic behavior across different data types without requiring explicit checks for each type within the calling code. By using conditional statements to determine the type of the object (`isNumber`, `isNative`, `isArray`, etc.), the function can delegate the execution of the method to the appropriate handler. This approach enhances maintainability and scalability by centralizing the logic for handling built-in methods.

For example, when dealing with numbers, the function checks if the method is `"toFixed"` or `"toString"`. For `"toFixed"`, it converts the number to a fixed-point notation string, allowing the user to specify the number of decimal places. For `"toString"`, it simply converts the number to a string representation.

When handling native objects, the function checks if the object is an instance of a class named `"str"` and if the method is `"maketrans"`. In this case, it creates a translation table dictionary where keys are characters from the first string and values are corresponding characters from the second string. This functionality is particularly useful for string manipulation tasks.

## Parameters/Return Value

### Parameters

- `obj`: A `QuantumValue` object representing the target object on which the method will be invoked.
- `method`: A `std::string` representing the name of the method to be called.
- `args`: A `std::vector<QuantumValue>` containing the arguments to pass to the method.

### Return Value

- Returns a `QuantumValue` object representing the result of the method call. If the method is not found or cannot be executed, it may return an empty `QuantumValue`.

## Edge Cases

1. **Invalid Method Name**: If the provided method name is not recognized for the given object type, the function should handle this gracefully, possibly returning an error message or an empty `QuantumValue`.
   
2. **Empty Arguments**: When invoking methods like `"toFixed"`, the function handles cases where no arguments are provided by defaulting to zero decimal places. Similarly, for other methods, it ensures proper handling of empty argument vectors.

3. **Type Mismatch**: The function should validate that the provided arguments match the expected types for the method being called. If there is a mismatch, it should return an appropriate error or exception.

4. **Class Hierarchy**: For instance objects, the function searches up the class hierarchy to find the method, ensuring that inherited methods can be accessed.

## Interactions With Other Components

- **Object Type Checking**: The function interacts with the VM core's data type checking mechanisms to determine the type of the object (`isNumber`, `isNative`, etc.). These checks ensure that the method is invoked on the correct type of object.

- **Method Invocation**: Depending on the object type, the function invokes either a specialized handler (e.g., `callArrayMethod`, `callStringMethod`) or a generic handler for native objects. These handlers manage the actual execution of the method.

- **Translation Table Creation**: For native objects of type `"str"` and method `"maketrans"`, the function creates a translation table dictionary, which is then returned as the result of the method call.

- **Error Handling**: Throughout the function, there is implicit error handling, especially in cases where method arguments do not meet expectations or the method itself is not supported. This ensures robustness and reliability in the VM core.

In summary, the `callBuiltinMethod` function provides a flexible and efficient mechanism for executing built-in methods on various data types within the Quantum Language compiler's VM core. Its design promotes code reusability, maintainability, and scalability, making it a vital component of the compiler's architecture.