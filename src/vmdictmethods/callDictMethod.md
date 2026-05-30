# `callDictMethod`

## Purpose

The `callDictMethod` function in the Quantum Language compiler's virtual machine (VM) is designed to handle various methods that can be invoked on a dictionary object. These methods include retrieving keys, values, items, checking for the presence of keys, getting and setting values, deleting entries, clearing the dictionary, and obtaining its size.

## Parameters

- `dict`: A shared pointer to a dictionary object (`std::shared_ptr<Dictionary>`).
- `m`: The name of the method to be called as a string (`std::string`).
- `args`: A vector of arguments passed to the method (`std::vector<QuantumValue>`).

## Return Value

- Returns a `QuantumValue` representing the result of the method invocation.

## Edge Cases

1. **Empty Arguments**: For methods like `"has"`, `"contains"`, and `"hasOwnProperty"`, an empty argument list returns `false`.
2. **Non-existent Key**: When calling `"get"` with a non-existent key, if a default value is provided, it returns the default value; otherwise, it returns an empty `QuantumValue`.
3. **Invalid Method Name**: If the method name is not recognized, it throws a `TypeError`.

## Interactions with Other Components

- **Dictionary Class**: This function interacts directly with the `Dictionary` class to perform operations such as retrieving keys, values, items, and checking for key existence.
- **Array Class**: It uses the `Array` class to create arrays that store the results of certain methods, such as `"keys"`, `"values"`, and `"items"`.
- **QuantumValue Class**: The function returns results wrapped in a `QuantumValue` object, which is used throughout the VM to represent data values.

## Implementation Details

Here’s how each method is implemented:

### `"keys"`

- **Purpose**: Retrieves all keys from the dictionary.
- **Implementation**:
  - Creates a new `Array` object.
  - Iterates over the dictionary using range-based for loop.
  - Pushes each key into the array as a `QuantumValue`.
  - Returns the array containing all keys.

### `"values"`

- **Purpose**: Retrieves all values from the dictionary.
- **Implementation**:
  - Creates a new `Array` object.
  - Iterates over the dictionary using range-based for loop.
  - Pushes each value into the array.
  - Returns the array containing all values.

### `"items"` / `"entries"`

- **Purpose**: Retrieves all key-value pairs from the dictionary.
- **Implementation**:
  - Creates a new `Array` object.
  - Iterates over the dictionary using range-based for loop.
  - For each key-value pair, creates a new `Array` object, pushes the key and value into it, and then pushes this pair array into the main array.
  - Returns the array containing all key-value pairs.

### `"has"` / `"contains"` / `"hasOwnProperty"`

- **Purpose**: Checks if the dictionary contains a specified key.
- **Implementation**:
  - If no arguments are provided, returns `false`.
  - Converts the first argument to a string and checks if the dictionary contains this key using `count`.
  - Returns `true` if the key exists, otherwise `false`.

### `"get"`

- **Purpose**: Retrieves the value associated with a specified key.
- **Implementation**:
  - If no arguments are provided, returns an empty `QuantumValue`.
  - Converts the first argument to a string and attempts to find this key in the dictionary.
  - If found, returns the corresponding value; otherwise, returns the second argument if provided, or an empty `QuantumValue` if not.

### `"set"`

- **Purpose**: Sets the value associated with a specified key.
- **Implementation**:
  - If at least two arguments are provided, converts the first argument to a string and assigns the second argument as the value in the dictionary.
  - Returns the updated dictionary wrapped in a `QuantumValue`.

### `"delete"`

- **Purpose**: Deletes an entry from the dictionary based on a specified key.
- **Implementation**:
  - If an argument is provided, converts it to a string and erases the corresponding entry from the dictionary.
  - Always returns `QuantumValue(true)` indicating deletion was successful.

### `"clear"`

- **Purpose**: Clears all entries from the dictionary.
- **Implementation**:
  - Calls the `clear` method on the dictionary.
  - Returns an empty `QuantumValue`.

### `"size"` / `"length"`

- **Purpose**: Retrieves the number of entries in the dictionary.
- **Implementation**:
  - Returns the size of the dictionary as a double wrapped in a `QuantumValue`.

## Example Usage

```cpp
// Assuming 'vm' is an instance of the VirtualMachine class
auto dict = vm.createDictionary();
dict->set(vm.createString("key1"), vm.createNumber(42));
dict->set(vm.createString("key2"), vm.createBoolean(true));

// Retrieve keys
auto keys = vm.callDictMethod(dict, "keys");
// keys will contain ["key1", "key2"]

// Get value
auto value = vm.callDictMethod(dict, "get", {vm.createString("key1")});
// value will be 42

// Set value
auto updatedDict = vm.callDictMethod(dict, "set", {vm.createString("key1"), vm.createNumber(99)});
// updatedDict will contain {"key1": 99, "key2": true}

// Delete entry
auto deleted = vm.callDictMethod(dict, "delete", {vm.createString("key1")});
// deleted will be true
// dict will now contain {"key2":