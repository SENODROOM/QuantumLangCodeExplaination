# `writeValue`

The `writeValue` function is a crucial method in the Quantum Language compiler's serialization process. Its primary responsibility is to convert a given value (`val`) into a binary format and write it to an output stream (`out`). This ensures that all types of values can be serialized efficiently and accurately.

## Parameters

- `out`: A reference to an output stream where the serialized data will be written.
- `val`: The value to be serialized. This could be of any type supported by the Quantum Language compiler, such as nil, boolean, number, string, array, or function.

## Return Value

This function does not return any value explicitly. Instead, it writes the serialized data directly to the output stream.

## How It Works

The `writeValue` function uses conditional checks to determine the type of the input value (`val`). Depending on the type, it performs different operations:

1. **Nil**: If the value is nil, the function writes a raw byte representing the `ValueType::VAL_NIL`.
2. **Boolean**: If the value is a boolean, it first writes a raw byte indicating `ValueType::VAL_BOOL`. Then, it writes another raw byte (either 1 for true or 0 for false).
3. **Number**: For numeric values, the function writes a raw byte denoting `ValueType::VAL_NUMBER`, followed by the actual numerical value.
4. **String**: Strings are serialized by writing a raw byte for `ValueType::VAL_STRING`, and then calling the `writeString` function to handle the string data.
5. **Array**: Arrays are serialized by writing a raw byte for `ValueType::VAL_ARRAY`, followed by the size of the array as a 32-bit unsigned integer. Each element in the array is then recursively serialized using the `writeValue` function.
6. **Function**: Functions are serialized by writing a raw byte for `ValueType::VAL_CLOSURE`, and then calling the `writeChunk` function to serialize the chunk associated with the function.

If the input value is of an unsupported type, the function throws a runtime error indicating the inability to serialize the specified type.

## Edge Cases

- **Nil**: The function handles nil values gracefully by simply writing a single byte.
- **Boolean**: Both true and false are represented correctly using a single byte each.
- **Number**: The function assumes that the numerical value fits within the range representable by its type. If the value exceeds this range, it may result in incorrect deserialization.
- **String**: Long strings might cause issues due to memory constraints when serializing their length.
- **Array**: Large arrays could lead to performance bottlenecks during serialization and deserialization.
- **Function**: The function relies on the `writeChunk` method to handle the serialization of chunks, which must also manage potential edge cases like large code sizes.

## Interactions with Other Components

- **Serialization Manager**: The `writeValue` function is part of a larger serialization system managed by the Serialization Manager. This manager coordinates the serialization process and ensures consistency across different parts of the compiler.
- **Output Stream**: The function interacts directly with the output stream provided by the caller. It writes bytes to this stream, which is later used to reconstruct the original values during deserialization.
- **Type Handling**: The function uses various type-specific methods (`writeRaw`, `writeString`, etc.) to handle different data types. These methods ensure that each type is serialized correctly without losing information.
- **Error Handling**: In case of an unsupported type, the function throws an exception, which is caught and handled by the higher-level serialization logic. This helps maintain robustness in the face of unexpected inputs.