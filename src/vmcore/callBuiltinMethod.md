# `callBuiltinMethod`

The `callBuiltinMethod` function is a crucial component of the Quantum Language compiler's virtual machine (VM) core. This function handles the invocation of built-in methods on various data types such as numbers, arrays, strings, dictionaries, and instances.

## What it Does

The primary purpose of `callBuiltinMethod` is to execute predefined built-in methods associated with different data types. These methods include:

- **Numbers**: Methods like `toFixed` and `toString`.
- **Arrays**: Array-specific methods.
- **Strings**: String-specific methods.
- **Dictionaries**: Dictionary-specific methods.
- **Instances**: Methods defined within classes or instances.

When a built-in method is invoked, the function checks the type of the object (`obj`) and then calls the appropriate method based on its type. If the method is not found, it throws a `TypeError`.

## Why it Works This Way

### Type Checking and Dispatching

The function uses a series of `if` statements to check the type of the object (`obj`). Depending on the type, it dispatches the method call to the corresponding handler function (`callArrayMethod`, `callStringMethod`, etc.). This approach ensures that each data type has its own set of methods and behaviors, maintaining encapsulation and modularity.

### Handling Built-In Methods

For numbers, the function supports two built-in methods:
- `toFixed`: Converts the number to a string with a specified number of decimal places. The default number of decimal places is 0 if none are provided.
- `toString`: Converts the number to a string representation.

These methods utilize the C++ standard library functions (`std::ostringstream`, `std::fixed`, `std::setprecision`) to perform the conversions.

### Method Resolution Order (MRO)

For instances, the function follows the Method Resolution Order (MRO) to find the method in the class hierarchy. It starts by checking the methods of the instance's class and then moves up to its base classes until it finds the method or reaches the root class. This ensures that inheritance is respected and that the most derived method is executed first.

### Field Access

If the method is not found in the class hierarchy, the function attempts to access a field with the same name using `inst->getField(method)`. This allows for dynamic attribute access similar to Python.

## Parameters/Return Value

### Parameters

- `obj`: The object on which the method will be invoked.
- `method`: A string representing the name of the method to invoke.
- `args`: A vector of `QuantumValue` objects representing the arguments passed to the method.
- `line`: An integer representing the current line number in the source code, used for error reporting.

### Return Value

The function returns a `QuantumValue` object, which represents the result of the method invocation. If an exception occurs during the method execution, it rethrows the exception.

## Edge Cases

### Negative Decimal Places

For the `toFixed` method, if a negative number of decimal places is provided, the function sets the number of decimal places to 0. This ensures that the output is always a non-negative number of decimal places.

### Non-existent Methods

If a method is not found in the object's class hierarchy or as a field, the function throws a `TypeError`. This provides clear feedback about the missing method and helps in debugging issues related to incorrect method names or invocations.

### Empty Arguments

For the `toFixed` method, if no arguments are provided, the function defaults to 0 decimal places. This handles cases where the user might forget to specify the number of decimal places.

## Interactions with Other Components

### Stack Management

The function interacts with the VM's stack through several operations:
- `push(obj)`: Pushes the object onto the stack before invoking the method.
- `push(a)`: Pushes each argument onto the stack.
- `pop()`: Pops the result of the method invocation from the stack after execution.

This stack management ensures that the method execution context is correctly maintained and that the results are properly returned to the caller.

### Frame Execution

After calling the closure associated with the method, the function executes the frame using `runFrame(savedDepth)`. This step is essential for handling the execution flow and managing local variables within the method.

### Error Reporting

The function uses the `line` parameter to report errors in a human-readable format. This helps in pinpointing the exact location in the source code where the error occurred, making debugging more efficient.

In summary, the `callBuiltinMethod` function is designed to handle the invocation of built-in methods on various data types within the Quantum Language compiler's VM core. Its modular design, type-specific handlers, and robust error reporting mechanisms ensure that the VM can effectively manage method calls and provide clear feedback in case of errors.