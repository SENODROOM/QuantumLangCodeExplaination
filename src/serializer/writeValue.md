# `writeValue`

The `writeValue` function is a critical method within the Quantum Language compiler's serialization process. Its main objective is to transform a provided value (`val`) into a binary format and subsequently write it to an output stream (`out`). This functionality is essential for preserving the state of the program during execution and enabling efficient data transfer between different parts of the system or across network boundaries.

## Parameters

- **`out`**: A reference to an output stream where the serialized value will be written. This could be any stream that supports binary writing, such as a file stream or a memory buffer.

## Return Value

This function does not return any value explicitly; instead, it writes the serialized data directly to the output stream.

## Edge Cases

1. **Nil Values**: If the input value is nil, the function writes a special marker indicating a nil value using the `ValueType::VAL_NIL`.
   
2. **Boolean Values**: For boolean values, the function first writes a marker indicating a boolean value using `ValueType::VAL_BOOL`. It then writes either a `0` or `1` to represent the false or true state, respectively.

3. **Numeric Values**: Numeric values are handled similarly to boolean values but with a different marker (`ValueType::VAL_NUMBER`). The actual numeric value is then written to the stream.

4. **String Values**: String values are marked with `ValueType::VAL_STRING`, followed by the length of the string encoded as a 32-bit unsigned integer. The string itself is then written to the stream.

5. **Array Values**: Arrays are marked with `ValueType::VAL_ARRAY`, followed by the size of the array encoded as a 32-bit unsigned integer. Each element in the array is then serialized recursively using the `writeValue` function.

6. **Function Values**: Functions are represented as closures, which are marked with `ValueType::VAL_CLOSURE`. The chunk associated with the closure, containing the bytecode and metadata, is serialized using the `writeChunk` function.

7. **Unsupported Types**: If the input value is of an unsupported type, the function throws a runtime error indicating the inability to serialize the specified type.

## Interactions with Other Components

The `writeValue` function interacts closely with several other components within the Quantum Language compiler:

- **Output Stream Management**: It utilizes the output stream (`out`) to manage binary data writing. The output stream must support binary operations to ensure correct serialization.

- **ValueType Enumerations**: The function uses enumerations defined in the `ValueType` namespace to identify the type of the input value. These enumerations help in distinguishing between different kinds of values and writing appropriate markers to the stream.

- **Helper Functions**: Several helper functions are used internally:
  - `writeRaw`: Writes raw binary data to the output stream.
  - `writeString`: Writes a string to the output stream, preceded by its length.
  - `writeChunk`: Writes a chunk of bytecode to the output stream.

These interactions allow the `writeValue` function to handle various data types efficiently and ensure that the serialized data is correctly formatted and stored.