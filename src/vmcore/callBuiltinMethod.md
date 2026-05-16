# `callBuiltinMethod`

The `callBuiltinMethod` function is a crucial part of the Quantum Language compiler's virtual machine (VM) core, responsible for executing built-in methods on different data types including numbers, arrays, strings, dictionaries, and instances.

## What it Does

The primary role of `callBuiltinMethod` is to handle the invocation of built-in methods based on the type of the object (`QuantumValue`) passed to it. Depending on whether the object is a number, a native object, an array, a string, a dictionary, or an instance, the function calls the appropriate helper function to process the method call.

### Number Type

For objects that represent numbers, `callBuiltinMethod` checks if the requested method is either `"toFixed"` or `"toString"`. 

- **`toFixed`**: If the method is `"toFixed"`, the function converts the number to a fixed-point notation string. The number of decimal places can be specified as an argument; if not provided, it defaults to `0`. If the specified number of decimal places is negative, it is set to `0`.
  
  Example:
  ```cpp
  QuantumValue num = QuantumValue(3.14159);
  QuantumValue result = callBuiltinMethod(num, "toFixed", {QuantumValue(2)});
  // result will be QuantumValue("3.14")
  ```

- **`toString`**: If the method is `"toString"`, the function simply returns the string representation of the number.

  Example:
  ```cpp
  QuantumValue num = QuantumValue(42);
  QuantumValue result = callBuiltinMethod(num, "toString");
  // result will be QuantumValue("42")
  ```

### Native Object Type

For native objects, which include special functions like `"str.maketrans"`, `"then"`, `"catch"`, and `"json"`, `callBuiltinMethod` performs the following actions:

- **`str.maketrans`**: This method creates a translation table for use with the `translate` method. It takes two arguments: `from` and `to`, which are strings representing characters to replace. The function constructs a dictionary where each character in `from` maps to its corresponding character in `to`.

  Example:
  ```cpp
  QuantumValue table = callBuiltinMethod(QuantumValue::createNativeObject("str"), "maketrans", 
                                        {QuantumValue("abc"), QuantumValue("xyz")});
  // table will be a Dict containing {"a": "x", "b": "y", "c": "z"}
  ```

- **`then`**, **`catch`**, and **`json`**: These methods are used for handling asynchronous operations and JSON serialization. For `"then"` and `"catch"`, if a function pointer (`native->fn`) is available, the function executes it with the provided arguments. For `"json"`, if no function pointer is available, it returns an empty dictionary.

  Example:
  ```cpp
  QuantumValue promise = QuantumValue::createPromise([](const std::vector<QuantumValue>& args){
      return QuantumValue("Resolved");
  });
  QuantumValue result = callBuiltinMethod(promise, "then", {});
  // result will be QuantumValue("Resolved")
  ```

### Array, String, Dictionary, and Instance Types

For arrays, strings, dictionaries, and instances, `callBuiltinMethod` delegates the handling to specialized helper functions (`callArrayMethod`, `callStringMethod`, `callDictMethod`, respectively). These functions process the method call according to the rules defined for their respective data types.

## Why it Works This Way

The design of `callBuiltinMethod` allows for a modular approach to handling built-in methods across different data types. By checking the type of the object and then calling the appropriate helper function, the code remains organized and easy to maintain. Each helper function encapsulates the logic for processing methods related to its specific data type, making the overall system more scalable and extensible.

## Parameters/Return Value

- **Parameters**:
  - `obj`: A `QuantumValue` object representing the target of the method call.
  - `method`: A `std::string` indicating the name of the method to be called.
  - `args`: A `std::vector<QuantumValue>` containing the arguments to pass to the method.

- **Return Value**:
  - Returns a `QuantumValue` object representing the result of the method call.

## Edge Cases

- **Negative Decimal Places in `toFixed`**: If a negative number is passed as the argument to `"toFixed"`, the function ensures that the number of decimal places is set to `0`.
- **Empty Arguments for `maketrans`**: If fewer than two arguments are provided to `"str.maketrans"`, the function handles this gracefully without throwing errors.
- **Missing Function Pointer for Asynchronous Methods**: If a function pointer is missing for methods like `"then"` and `"catch"`, the function provides default behavior (e.g., returning an empty dictionary).

## Interactions with Other Components

- **Virtual Machine Core**: `callBuiltinMethod` operates within the context of the VM core, interacting with other parts of the system to manage memory, execute functions, and handle data types.
- **Helper Functions**: The function relies on specialized helper functions (`callArrayMethod`, `callStringMethod`, etc.) to process method calls for specific data types.
- **Built-In Objects**: `callBuiltinMethod` interacts with built-in objects (like promises and translation tables), allowing these objects to perform specific tasks when their methods are invoked.

Overall, `callBuiltinMethod` plays a vital role in enabling the Quantum Language compiler