# `callBuiltinMethod`

The `callBuiltinMethod` function is an essential component of the Quantum Language compiler's virtual machine (VM) core, tasked with executing built-in methods across various data types such as numbers, arrays, strings, dictionaries, and instances. This function facilitates dynamic method invocation based on the type of the object and the specified method name, ensuring that operations like formatting numbers, converting objects to strings, handling asynchronous operations, and interacting with email functionalities can be performed efficiently.

## Parameters

- **obj**: A reference to the `QuantumObject` on which the built-in method should be executed. This object represents the data structure or instance on which the operation will take place.
- **method**: A string representing the name of the built-in method to be invoked. The method names are case-sensitive and must match exactly with the predefined built-in methods.
- **args**: A vector of `QuantumValue` representing the arguments passed to the built-in method. These arguments provide additional context or data required for the method execution.

## Return Value

The function returns a `QuantumValue`, which encapsulates the result of the built-in method execution. Depending on the method called and its parameters, the returned value could be a formatted number, a new string, a dictionary, an array, or even another instance of a class.

## Edge Cases

- **Invalid Method Name**: If the provided method name does not correspond to any built-in method supported by the current data type, the function may either throw an exception or return a default value, depending on the implementation.
- **Empty Arguments**: For some methods, such as `toFixed`, the absence of arguments might imply a default behavior (e.g., rounding to zero decimal places). However, for other methods that require arguments, passing an empty vector would likely lead to an error.
- **Unsupported Data Types**: If the object is of a type that does not support the requested method, the function will typically ignore the request and return the original object unchanged.

## Interactions with Other Components

- **Type Checking**: Before attempting to execute a method, `callBuiltinMethod` checks whether the object supports the specified method using conditional statements (`if (obj.isNumber())`, `if (obj.isNative())`, etc.). Each branch corresponds to a different data type, allowing for specialized handling of each type.
  
- **Method Execution**: Once the appropriate branch is identified, the function proceeds to execute the method. For example, if the object is a number and the method is `"toFixed"`, the function formats the number according to the specified precision and returns the formatted string wrapped in a `QuantumValue`.

- **Asynchronous Operations**: For certain built-in methods, such as those related to email handling (`receive_email`, `list_emails`, etc.), the function might initiate an asynchronous operation and return immediately, leaving the actual processing to be handled by separate threads or event loops within the VM core.

- **Error Handling**: Throughout the function, there is implicit error handling through the use of conditionals and assertions. For instance, when converting a number to a string, the function ensures that the number is non-negative before proceeding with the conversion. Similarly, when creating a translation table for strings, the function checks that both the source and target strings have sufficient length to avoid runtime errors.

In summary, `callBuiltinMethod` is a versatile function designed to handle a wide range of built-in operations across different data types in the Quantum Language compiler's VM core. Its careful design ensures robustness and efficiency, making it a crucial part of the system's functionality.