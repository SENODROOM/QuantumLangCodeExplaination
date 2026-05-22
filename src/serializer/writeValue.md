# `writeValue`

The `writeValue` function is a crucial method within the Quantum Language compiler's serialization process. Its primary objective is to convert a given value (`val`) into a binary format and then write it to an output stream (`out`). This functionality is vital for ensuring that values can be accurately stored and retrieved in a serialized form, which is often necessary for saving program states or transmitting data between different parts of the system or across network boundaries.

## Parameters

- `out`: A reference to an output stream where the serialized value will be written. This could be any stream that supports writing binary data, such as a file stream or a network socket stream.

- `val`: The value to be serialized. This value can be of various types including nil, boolean, number, string, array, and function.

## Return Value

This function does not return a value explicitly. Instead, it writes the serialized data directly to the output stream.

## How It Works

The `writeValue` function operates based on the type of the input value (`val`). Here’s how it handles each type:

1. **Nil**: If the value is `nil`, the function writes a raw byte representing the `ValueType::VAL_NIL`.

2. **Boolean**: For boolean values, the function first writes a raw byte indicating the type `ValueType::VAL_BOOL`. Then, it writes another raw byte, either `1` for `true` or `0` for `false`.

3. **Number**: When serializing numbers, the function starts by writing a raw byte denoting the type `ValueType::VAL_NUMBER`. Following this, it writes the actual numeric value using the appropriate binary representation (e.g., `int32_t`, `double`).

4. **String**: Strings are serialized by writing a raw byte for their type `ValueType::VAL_STRING` followed by the length of the string as a `uint32_t`. After that, the string itself is written to the output stream.

5. **Array**: Arrays are serialized by writing a raw byte for their type `ValueType::VAL_ARRAY`. They also include the size of the array as a `uint32_t`. Each element in the array is then recursively serialized using the `writeValue` function.

6. **Function**: Functions are serialized as closures, indicated by a raw byte for their type `ValueType::VAL_CLOSURE`. The function's chunk is then written to the output stream using the `writeChunk` function.

If the input value is of a type that the function cannot handle, it throws a `std::runtime_error` exception with a message indicating the unsupported type.

## Edge Cases

- **Empty Array**: An empty array is handled gracefully by simply writing a `uint32_t` value of `0` after the `ValueType::VAL_ARRAY` marker.

- **Nested Structures**: The function correctly handles nested structures by recursively calling itself to serialize each component of the structure.

- **Unsupported Types**: Any attempt to serialize a value of an unsupported type results in an error, ensuring robustness against unexpected inputs.

## Interactions With Other Components

The `writeValue` function interacts closely with several other components within the Quantum Language compiler:

- **Output Stream**: Directly uses the output stream to write binary data. This interaction is fundamental to its operation.

- **ValueType Enum**: Relies on the `ValueType` enum to identify the type of the value being serialized. This enumeration helps in distinguishing between different types of values.

- **writeRaw Function**: Utilizes the `writeRaw` function to write basic binary data types like bytes and integers. This function is likely part of a utility class that provides low-level operations for handling binary data.

- **writeString Function**: Invokes the `writeString` function when serializing strings. This function might handle encoding details, ensuring compatibility across different systems.

- **writeChunk Function**: Calls the `writeChunk` function when serializing functions. This function is responsible for writing the bytecode associated with the function, enabling the deserialized function to execute identically to the original.

Overall, the `writeValue` function plays a pivotal role in the serialization mechanism of the Quantum Language compiler, ensuring that all types of values are converted into a consistent binary format that can be easily transmitted or stored.