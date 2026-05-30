# `writeValue`

The `writeValue` function is a crucial method within the Quantum Language compiler's serialization process. Its primary objective is to convert a given value (`val`) into a binary format and then write it to an output stream (`out`). This functionality is vital for ensuring that values can be stored or transmitted efficiently and accurately across different parts of the system or even between systems.

## What It Does

The `writeValue` function serializes various types of values including nil, boolean, number, string, array, and closure. Each type is represented by a unique `ValueType`, which helps in distinguishing them during deserialization. The function writes these values to the provided output stream (`out`), ensuring they are stored in a binary format suitable for efficient data handling.

## Why It Works This Way

This approach allows for a flexible and extensible serialization mechanism. By using distinct `ValueType` identifiers, the function can handle different types of values without ambiguity. Writing each type in its own manner ensures that the serialized data is both compact and clear, facilitating easy parsing and reconstruction of the original values during deserialization.

## Parameters/Return Value

- **Parameters**:
  - `out`: A reference to the output stream where the serialized value will be written.
  - `val`: The value to be serialized.

- **Return Value**: None. The function directly writes to the output stream and does not return any value.

## Edge Cases

- **Nil Values**: If the input value is nil, the function writes a single byte representing `ValueType::VAL_NIL`.
- **Boolean Values**: For boolean values, the function writes two bytes: one indicating the type as `ValueType::VAL_BOOL`, and another byte representing either `1` (true) or `0` (false).
- **Number Values**: Number values are serialized by writing three bytes: one indicating the type as `ValueType::VAL_NUMBER`, followed by the actual number value.
- **String Values**: String values are serialized by first writing three bytes indicating the type as `ValueType::VAL_STRING`, followed by the length of the string and the string itself.
- **Array Values**: Array values are serialized by writing four bytes indicating the type as `ValueType::VAL_ARRAY`, followed by the size of the array and each element in the array.
- **Closure Values**: Closure values are serialized by writing five bytes indicating the type as `ValueType::VAL_CLOSURE`, followed by the chunk associated with the closure.

## Interactions With Other Components

The `writeValue` function interacts closely with several other components within the Quantum Language compiler:

- **Output Stream**: The function uses the provided output stream (`out`) to store the serialized data. This interaction is essential for persisting the data or transmitting it over a network.
  
- **ValueType Enum**: The function relies on the `ValueType` enum to identify the type of the input value. This enumeration provides a standardized way to represent different value types, making the serialization process consistent and error-free.

- **writeRaw Function**: The `writeValue` function calls `writeRaw` to write basic data types such as integers and booleans directly to the output stream. This helper function simplifies the serialization process by abstracting away the details of raw data writing.

- **writeString Function**: When serializing string values, the `writeValue` function calls `writeString`. This function handles the conversion of strings into their binary representation, including the length prefix.

- **writeChunk Function**: For closure values, the `writeValue` function calls `writeChunk` to serialize the chunk associated with the closure. This interaction ensures that all necessary data related to the closure is preserved during serialization.

In summary, the `writeValue` function plays a pivotal role in the Quantum Language compiler's serialization process by converting various types of values into a binary format and writing them to an output stream. Its design ensures flexibility, clarity, and efficient data handling, making it a robust component of the compiler's architecture.