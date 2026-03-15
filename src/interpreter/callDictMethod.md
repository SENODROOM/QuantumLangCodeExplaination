# `callDictMethod`

The `callDictMethod` function is designed to handle method calls on dictionary objects within the Quantum Language compiler's interpreter. This function allows for various operations on dictionaries such as checking for keys, retrieving values, setting new key-value pairs, deleting entries, clearing the entire dictionary, adding entries, and iterating over the dictionary.

## What It Does

- **Checks for Key Existence (`"has"`)**: Determines whether a specified key exists in the dictionary.
- **Retrieves Value (`"get"`)**: Returns the value associated with a given key. If the key does not exist, it returns either the default value provided or an empty `QuantumValue`.
- **Sets New Key-Value Pair (`"set"`)**: Adds a new key-value pair to the dictionary or updates the existing value of a key.
- **Deletes Entry (`"delete"`)**: Removes a key-value pair from the dictionary based on the specified key.
- **Clears Dictionary (`"clear"`)**: Empties the entire dictionary.
- **Adds Entry (`"add"`)**: Adds a new entry to the dictionary where both the key and value are the same.
- **Returns Dictionary Size (`"size"`)**: Provides the number of key-value pairs currently stored in the dictionary.
- **Returns All Values (`"values"`)**: Retrieves all values from the dictionary and returns them as an array.
- **Returns All Keys (`"keys"`)**: Retrieves all keys from the dictionary and returns them as an array.
- **Returns All Entries (`"entries"`)**: Retrieves all key-value pairs from the dictionary and returns them as an array of arrays.
- **Iterates Over Dictionary (`"forEach"`)**: Allows for iteration over each key-value pair in the dictionary using a callback function.

## Why It Works This Way

The design of `callDictMethod` follows a straightforward approach to provide comprehensive functionality for dictionary manipulation. Each method corresponds to a common operation that can be performed on a dictionary, ensuring that developers have access to essential tools for managing their data structures efficiently.

### Edge Cases

- **Empty Arguments**: For methods like `"get"`, `"delete"`, and `"add"`, handling empty arguments appropriately prevents runtime errors.
- **Key Not Found**: In the case of the `"get"` method, when the key is not found, it checks if a default value has been provided; otherwise, it returns an empty `QuantumValue`.
- **Invalid Callback**: The `"forEach"` method ensures that the provided callback is valid before attempting to execute it, preventing potential crashes due to invalid function pointers.

## Parameters/Return Value

### Parameters

- `m`: A string representing the method name to be called.
- `args`: A vector of `QuantumValue` objects containing the arguments passed to the method.

### Return Value

- Returns a `QuantumValue` object which encapsulates the result of the method call. The type of the returned value depends on the method being executed:
  - Boolean for `"has"` and `"delete"`.
  - Double for `"size"`.
  - Shared pointer to an `Array` for `"values"`, `"keys"`, and `"entries"`.
  - Void for `"set"` and `"clear"`.
  - Original dictionary for `"add"` and `"forEach"`.

## Interactions With Other Components

- **Dictionary Object (`dict`)**: The primary component interacted with by `callDictMethod`. It is a shared pointer to a `std::map<std::string, QuantumValue>` that holds the key-value pairs.
- **Callback Function (`args[0]`)**: Used in the `"forEach"` method to allow external code to iterate over the dictionary entries. The callback must be a native function, and its execution involves passing the current value and key as arguments.

This function serves as a crucial interface for dictionary operations within the Quantum Language compiler, facilitating efficient and flexible management of data structures.