# `callDictMethod`

## Purpose

The `callDictMethod` function in the Quantum Language compiler's virtual machine (VM) is designed to handle various methods that can be invoked on a dictionary object. These methods include retrieving keys, values, items, checking for the presence of keys, getting and setting key-value pairs, deleting entries, clearing the entire dictionary, and determining its size.

## Implementation Details

### Parameters
- `m`: A string representing the name of the method to be called.
- `args`: A vector of `QuantumValue` objects representing the arguments passed to the method.

### Return Value
- Returns a `QuantumValue` object containing the result of the method call or an error if the method is not recognized.

### How It Works

The function uses a series of conditional checks to determine which method was requested based on the input string `m`. Depending on the method, it performs the following actions:

1. **Keys Method**: If `m` is `"keys"`, the function iterates over all keys in the dictionary and returns them as an array of `QuantumValue` objects.
   
2. **Values Method**: If `m` is `"values"`, the function iterates over all values in the dictionary and returns them as an array of `QuantumValue` objects.
   
3. **Items/Entries Method**: If `m` is either `"items"` or `"entries"`, the function iterates over all key-value pairs in the dictionary. For each pair, it creates a nested array containing the key and the value, then pushes this nested array into an outer array. Finally, it returns the outer array as a `QuantumValue`.
   
4. **Has/Contains/HasOwnProperty Methods**: If `m` matches any of these strings, the function checks whether the first argument (`args[0]`) exists as a key in the dictionary. It returns a boolean `QuantumValue` indicating the presence of the key.
   
5. **Get Method**: If `m` is `"get"`, the function retrieves the value associated with the first argument (`args[0]`). If the key is found, it returns the corresponding value; otherwise, it returns the second argument if provided, or an empty `QuantumValue` if not.
   
6. **Set Method**: If `m` is `"set"`, the function updates the value associated with the first argument (`args[0]`) to the second argument (`args[1]`). If both arguments are provided, it sets the new value and returns the updated dictionary as a `QuantumValue`; otherwise, it throws an error.
   
7. **Delete Method**: If `m` is `"delete"`, the function removes the entry associated with the first argument (`args[0]`) from the dictionary. If the key exists, it erases the entry and returns a `QuantumValue` with the boolean `true`; otherwise, it returns `true` without making any changes.
   
8. **Clear Method**: If `m` is `"clear"`, the function removes all entries from the dictionary. It clears the dictionary and returns an empty `QuantumValue`.
   
9. **Size/Length Method**: If `m` is either `"size"` or `"length"`, the function returns the number of entries in the dictionary as a double `QuantumValue`.

If none of the above conditions match, the function throws a `TypeError` indicating that the dictionary has no such method.

## Edge Cases

- **Empty Dictionary**: When calling methods like `"keys"`, `"values"`, `"items"`, or `"entries"` on an empty dictionary, the function will return an empty array.
- **Non-existent Key**: When calling `"get"` or `"has"` with a non-existent key, the function will return the default value or `false`, respectively.
- **Incorrect Number of Arguments**: When calling `"set"`, `"get"`, or `"delete"` with insufficient arguments, the function will throw an error.
- **Invalid Method Name**: If an unrecognized method name is provided, the function will throw a `TypeError`.

## Interactions with Other Components

The `callDictMethod` function interacts with the `Dictionary` class and the `Array` class within the VM. The `Dictionary` class provides the underlying data structure for storing key-value pairs, while the `Array` class is used to create arrays of keys, values, and key-value pairs. The function also utilizes the `QuantumValue` class to represent the results and arguments, ensuring consistency across different parts of the VM.

In summary, the `callDictMethod` function serves as a versatile interface for performing common operations on dictionary objects within the Quantum Language compiler's VM, handling various scenarios and providing appropriate results or errors.