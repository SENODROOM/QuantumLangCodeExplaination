# `writeValue`

The `writeValue` function is a critical method within the Quantum Language compiler's serialization process. Its primary objective is to convert a given value (`val`) into a binary format and then write it to an output stream (`out`). This functionality is vital for ensuring that values can be stored or transmitted efficiently and accurately between different parts of the compiler or even across different systems.

## What It Does

The `writeValue` function serializes various types of values such as nil, boolean, number, string, array, and closure. Each type is represented by a unique `ValueType`, which is written to the output stream first. Depending on the type, additional data is encoded and written:

- **Nil**: Writes a single byte representing the `ValueType::VAL_NIL`.
- **Boolean**: Writes a single byte representing the `ValueType::VAL_BOOL` followed by a single byte indicating whether the boolean is true (1) or false (0).
- **Number**: Writes a single byte representing the `ValueType::VAL_NUMBER` followed by the actual numeric value.
- **String**: Writes a single byte representing the `ValueType::VAL_STRING` followed by the length of the string and the string itself.
- **Array**: Writes a single byte representing the `ValueType::VAL_ARRAY` followed by the size of the array and each element in the array serialized recursively.
- **Closure**: Writes a single byte representing the `ValueType::VAL_CLOSURE` followed by the chunk associated with the closure.

If the input value is of an unsupported type, the function throws a runtime error.

## Why It Works This Way

This design ensures that the serialized data is both compact and easily interpretable. By using a discriminator (`ValueType`), the deserialization process knows how to interpret each segment of the binary data correctly. The recursive nature of handling arrays allows for nested structures to be serialized properly without requiring additional metadata about the structure.

## Parameters/Return Value

### Parameters

- `out`: A reference to the output stream where the serialized data will be written.
- `val`: The value to be serialized.

### Return Value

No return value; the function writes directly to the output stream.

## Edge Cases

- **Nil Values**: The function handles nil values gracefully by writing a single byte.
- **Unsupported Types**: If the input value is of a type not explicitly handled, the function throws an exception.
- **Empty Arrays**: When serializing empty arrays, the function writes the size as zero, indicating no elements follow.

## Interactions With Other Components

The `writeValue` function interacts with several other components within the serializer:

- **writeRaw**: Used to write basic data types like bytes or numbers directly to the output stream.
- **writeString**: Handles the serialization of strings, including their length and content.
- **writeChunk**: For closures, it writes the chunk associated with the function, which contains the bytecode and constants.

These functions collectively ensure that all necessary information about the value is captured and stored in a binary format suitable for later deserialization.