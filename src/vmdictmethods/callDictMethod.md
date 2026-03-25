# `callDictMethod`

## Purpose

The `callDictMethod` function is designed to handle various methods that can be called on a dictionary object within the Quantum Language compiler's virtual machine (VM). This function processes different method calls such as retrieving keys, values, items, checking for the presence of an item, setting or deleting items, and clearing the entire dictionary.

## Parameters

- `dict`: A pointer to a shared `Dictionary` object. This represents the dictionary on which the method is being called.
- `m`: A string representing the name of the method to be executed.
- `args`: A vector of `QuantumValue` objects representing the arguments passed to the method.

## Return Value

- The function returns a `QuantumValue` object, which could represent various types of data depending on the method called:
  - An array containing keys (`"keys"` method).
  - An array containing values (`"values"` method).
  - An array of arrays containing key-value pairs (`"items"` or `"entries"` method).
  - A boolean indicating whether the dictionary contains a specified key (`"has"`, `"contains"`, or `"hasOwnProperty"` method).
  - The value associated with a specified key (`"get"` method).
  - The modified dictionary after setting a new key-value pair (`"set"` method).
  - A boolean indicating success (`true`) or failure (`false`) of the delete operation (`"delete"` method).
  - An empty `QuantumValue` object if the dictionary is cleared (`"clear"` method).
  - A double representing the size of the dictionary (`"size"` or `"length"` method).

## Edge Cases

- If the method name `m` is not recognized, the function throws a `TypeError`.
- For the `"get"` method, if the dictionary is empty or the specified key does not exist, it returns either an empty `QuantumValue` or a default value provided as the second argument in `args`.
- For the `"set"` method, if fewer than two arguments are provided, it does nothing and returns the original dictionary.
- For the `"delete"` method, if no arguments are provided, it does nothing and returns `true`.

## Interactions with Other Components

- **Dictionary Class**: The function interacts directly with the `Dictionary` class, which is assumed to have methods like `count`, `find`, `erase`, and `clear`. These methods are used to perform operations such as counting occurrences of a key, finding a key's value, removing a key-value pair, and clearing all entries in the dictionary.
  
- **Array Class**: When returning results, the function uses the `Array` class to store collections of data. This includes creating arrays of keys, values, or key-value pairs.

- **QuantumValue Class**: The function utilizes the `QuantumValue` class to encapsulate the result of each method call. This class likely handles different data types and conversions necessary for the VM's internal representation.

## Implementation Details

The function employs a series of conditional checks to determine which method to execute based on the input string `m`. Each case corresponds to a specific dictionary method:

- **Keys Method**: Iterates through the dictionary and pushes each key into a new `Array`, then returns this array wrapped in a `QuantumValue`.
- **Values Method**: Similar to the keys method but pushes each value instead of the key.
- **Items/Entries Method**: Creates an array for each key-value pair, pushing both the key and its corresponding value into the sub-array. All these sub-arrays are then pushed into a main array, which is returned.
- **Has/Contains/HasOwnProperty Method**: Checks if the dictionary contains the specified key. Returns `true` if found, otherwise `false`.
- **Get Method**: Retrieves the value associated with the specified key. If the key does not exist, it returns either an empty `QuantumValue` or a default value if provided.
- **Set Method**: Sets a new key-value pair in the dictionary. It requires at least two arguments: the key and the value. If successful, it returns the modified dictionary.
- **Delete Method**: Removes the specified key-value pair from the dictionary. If no key is specified, it does nothing. It always returns `true`.
- **Clear Method**: Clears all entries from the dictionary. It returns an empty `QuantumValue`.

Each method is implemented using appropriate STL containers and algorithms, ensuring efficient data manipulation and retrieval. The use of smart pointers (`std::shared_ptr`) helps manage memory effectively, preventing potential leaks and dangling references.

This function is crucial for enabling dynamic interaction with dictionary objects within the Quantum Language compiler's VM, providing essential functionality for developers working with these data structures.