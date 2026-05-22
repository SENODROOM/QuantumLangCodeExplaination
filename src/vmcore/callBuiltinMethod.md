# `callBuiltinMethod`

The `callBuiltinMethod` function is an integral part of the Quantum Language compiler's virtual machine (VM) core, responsible for executing built-in methods on different data types including numbers, arrays, strings, dictionaries, and instances. This function ensures dynamic method invocation within the VM, allowing for flexible and efficient execution of predefined operations.

## Parameters

- **obj**: A `QuantumValue` object representing the instance or data type on which the method will be called.
- **method**: A string indicating the name of the built-in method to be executed.
- **args**: A vector of `QuantumValue` objects representing the arguments passed to the method.

## Return Value

The function returns a `QuantumValue` object containing the result of the method execution. The returned value can be of any data type supported by the Quantum Language, such as numbers, strings, dictionaries, or even other instances.

## Edge Cases

### Invalid Method Name

If the provided `method` parameter does not correspond to any valid built-in method for the given `obj`, the function will return the original `obj`. For example:

```cpp
QuantumValue result = callBuiltinMethod(numberObj, "invalidMethod", {});
// result remains numberObj
```

### Negative Precision for Numbers

When calling the `toFixed` method on a number, if a negative precision value is provided, the function will set the precision to zero. For example:

```cpp
QuantumValue result = callBuiltinMethod(numberObj, "toFixed", { -1 });
// result is equivalent to calling toFixed with 0 precision
```

### Empty Arguments for String Methods

For methods that do not require arguments, providing an empty `args` vector will still invoke the method successfully. For example:

```cpp
QuantumValue result = callBuiltinMethod(stringObj, "toString", {});
// result is equivalent to calling toString without arguments
```

## Interactions with Other Components

### Number Operations

For number objects, `callBuiltinMethod` handles methods like `toFixed` and `toString`. These methods utilize standard C++ libraries (`std::ostringstream` and `std::setprecision`) to format and convert the number accordingly.

### Native Object Methods

For native objects, the function checks if the method corresponds to specific built-in functionalities such as `maketrans` for creating translation tables, `then`, `catch`, and `json` for handling asynchronous operations and JSON serialization. If a matching method is found, it invokes the corresponding function pointer stored in the native object.

### Array Operations

If the provided `obj` is an array, `callBuiltinMethod` delegates the method call to the `callArrayMethod` function, which processes array-specific operations.

### String Operations

Similarly, for string objects, `callBuiltinMethod` forwards the method call to the `callStringMethod` function, handling string-specific operations like `maketrans`.

### Dictionary Operations

For dictionary objects, the function calls the `callDictMethod` function, which manages dictionary-specific operations.

### Instance Operations

For instance objects, `callBuiltinMethod` searches through the class hierarchy to find the specified method. Once located, it executes the method using the appropriate instance data.

In summary, the `callBuiltinMethod` function provides a robust mechanism for invoking built-in methods on various data types within the Quantum Language VM core. Its design ensures flexibility, efficiency, and ease of integration with other components of the compiler, thereby enhancing the overall functionality and usability of the language.