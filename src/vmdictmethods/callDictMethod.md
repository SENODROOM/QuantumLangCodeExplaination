# `callDictMethod`

## Purpose

The `callDictMethod` function in the Quantum Language compiler's virtual machine (VM) is designed to handle various methods that can be invoked on a dictionary object. These methods include retrieving keys, values, items, checking for the presence of keys, getting and setting values, deleting entries, clearing the dictionary, and determining its size.

## Parameters

- `dict`: A shared pointer to the `Dictionary` object on which the method is being called.
- `m`: A string representing the name of the method to be invoked.
- `args`: A vector of `QuantumValue` objects representing the arguments passed to the method.

## Return Value

The function returns a `QuantumValue` object containing the result of the method invocation. The type of the returned value depends on the method being called:

- For methods returning arrays (`keys`, `values`, `items`, `entries`), it returns a `QuantumValue` containing a shared pointer to an `Array`.
- For methods returning boolean values (`has`, `contains`, `hasOwnProperty`), it returns a `QuantumValue` containing either `true` or `false`.
- For methods returning numeric values (`size`, `length`), it returns a `QuantumValue` containing a double representing the size of the dictionary.
- For methods that modify the dictionary (`set`, `delete`, `clear`), it returns a `QuantumValue` containing the modified dictionary or a boolean indicating success.

## Edge Cases

- If the method name `m` does not correspond to any valid dictionary method, the function throws a `TypeError`.
- When invoking the `get` method without any arguments, the function returns an empty `QuantumValue`.
- When invoking the `get` method with one argument but no default value provided, the function returns `QuantumValue()` if the key is not found.
- When invoking the `get` method with two arguments, the function returns the second argument as the default value if the key is not found.
- When invoking the `set` method with fewer than two arguments, the function does nothing and returns `QuantumValue(dict)`.

## Interactions with Other Components

The `callDictMethod` function interacts with several other components within the VM:

- **Dictionary Class**: It uses methods like `count`, `find`, and `erase` of the `Dictionary` class to perform operations such as checking for key existence, finding values, and deleting entries.
- **Array Class**: It creates instances of the `Array` class to store the results of methods like `keys`, `values`, and `items`. Each element in the array corresponds to a key-value pair in the dictionary.
- **QuantumValue Class**: It constructs `QuantumValue` objects to represent the results of method invocations, including both primitive types (like booleans and doubles) and complex types (like arrays).

This function ensures that dictionary operations are encapsulated within the VM, providing a consistent interface for handling dictionary-related tasks across different parts of the compiler.