# `readValue` Function

## Overview

The `readValue` function is an essential component of the Quantum Language compiler's serialization system. Its main purpose is to reconstruct values from serialized data, ensuring that complex data structures and their contents are accurately restored during the deserialization process.

### Why It Works This Way

The function operates by first reading the raw value type from the serialized data using `readRaw<ValueType>`. Depending on the type, it then reads the corresponding data and constructs a `QuantumValue` object accordingly. This approach allows for the dynamic reconstruction of various types of values, including nil, booleans, numbers, strings, arrays, and closures.

## Parameters/Return Value

- **Parameters**:
  - `const char* data`: A pointer to the serialized data.
  - `size_t& offset`: A reference to the current offset in the data stream, which is updated as the function processes each value.

- **Return Value**:
  - Returns a `QuantumValue` object representing the reconstructed value.

## Edge Cases

1. **Nil Type**: If the value type is `VAL_NIL`, the function returns an empty `QuantumValue`.
2. **Boolean Type**: For boolean values, the function checks if the raw byte is non-zero and returns a `QuantumValue` containing `true` or `false`.
3. **Number Type**: The function directly reads a double-precision floating-point number and returns a `QuantumValue` containing this number.
4. **String Type**: The function calls `readString` to reconstruct the string from the serialized data and returns a `QuantumValue` containing the string.
5. **Array Type**: When encountering an array, the function reads the array size, creates a shared pointer to an `Array` object, and recursively reads each element into the array. Finally, it returns a `QuantumValue` containing the constructed array.
6. **Closure Type**: For closures, the function reads a chunk of serialized data using `readChunk` and constructs a `Closure` object from this chunk. It then returns a `QuantumValue` containing the closure.

## Interactions With Other Components

- **`readRaw<T>`**: This helper function reads a value of type `T` from the serialized data at the specified offset and updates the offset.
- **`readString`**: This function reconstructs a string from the serialized data starting at the given offset and updates the offset accordingly.
- **`readChunk`**: This function reads a chunk of serialized data starting at the given offset and updates the offset. The chunk represents a portion of the serialized data that can be used to construct more complex objects like closures.

These functions work together to ensure that the entire serialized data structure is correctly reconstructed during the deserialization process.