# `callBuiltinMethod`

The `callBuiltinMethod` function is an essential component of the Quantum Language compiler's virtual machine (VM) core, designed to execute built-in methods across various data types such as numbers, arrays, strings, dictionaries, and instances. This function facilitates dynamic method invocation based on the type of the object and the specified method name.

## What it Does

The `callBuiltinMethod` function evaluates the type of the provided `QuantumValue` object (`obj`) and then checks if the requested method exists within that type's built-in capabilities. If the method is found, the function executes it with the given arguments (`args`). The result of the method execution is returned as a new `QuantumValue`.

For example, when called with a number object and the method `"toFixed"`, it converts the number to a string with a fixed number of decimal places. Similarly, calling it with a string object and the method `"toUpperCase"` would convert all characters in the string to uppercase.

## Why it Works This Way

This design allows the VM core to handle different data types uniformly while providing access to built-in functionalities through method calls. By checking the type of the object and invoking the appropriate method, the function ensures that the correct operation is performed without requiring explicit type-specific code paths. This approach enhances maintainability and scalability of the compiler.

## Parameters

- `obj`: A `QuantumValue` representing the object on which the built-in method should be executed.
- `method`: A `std::string` specifying the name of the built-in method to invoke.
- `args`: A `std::vector<QuantumValue>` containing the arguments to pass to the method.

## Return Value

The function returns a `QuantumValue` representing the result of the built-in method execution. If the method does not exist or cannot be invoked, it may return a default value or throw an exception.

## Edge Cases

1. **Invalid Method Name**: If the provided method name does not correspond to any built-in method for the object's type, the function should handle this gracefully, either by returning a default value or throwing an error.
2. **Type Mismatch**: Ensure that the method arguments match the expected types for the method being invoked. For instance, a method expecting a number might fail if passed a string argument.
3. **Empty Arguments**: Handle cases where the method expects arguments but none are provided. Default values can be used for optional arguments.

## Interactions with Other Components

- **Object Type Checking**: The function interacts with the `isNumber()`, `isNative()`, `isArray()`, `isString()`, `isDict()`, and `isInstance()` methods to determine the type of the `obj`.
- **Method Invocation**: Depending on the object type, it invokes either `callArrayMethod`, `callStringMethod`, `callDictMethod`, or directly executes the method on the instance using `inst->klass.get()->findMethod(method)`.

In summary, the `callBuiltinMethod` function is a versatile mechanism within the Quantum Language compiler's VM core, enabling the execution of built-in methods across various data types. Its implementation leverages polymorphism and type-specific handling to provide a unified interface for method invocation, enhancing both efficiency and ease of maintenance.