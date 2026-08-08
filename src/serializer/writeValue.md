# `writeValue`

The `writeValue` function is a critical method within the Quantum Language compiler's serialization process. Its primary objective is to convert a given value (`val`) into a binary format and then write it to an output stream (`out`). This functionality is vital for ensuring that values can be persisted or transmitted efficiently between different parts of the system or across network boundaries.

## Parameters/Return Value

- **Parameters**:
  - `out`: A reference to an output stream where the serialized value will be written.
  - `val`: The value to be serialized.

- **Return Value**:
  - None. The function writes directly to the output stream.

## How It Works

The `writeValue` function handles different types of values by checking their type using methods like `isNil`, `isBool`, `isNumber`, etc. For each type, it performs the following steps:

1. **Nil Values**: If the value is nil (`isNil()`), it writes a raw byte representing the nil type (`ValueType::VAL_NIL`) to the output stream.
   
2. **Boolean Values**: If the value is a boolean (`isBool()`), it first writes a raw byte indicating the boolean type (`ValueType::VAL_BOOL`). Then, it writes another byte that represents the boolean value itself (1 for true, 0 for false).

3. **Numeric Values**: For numeric values (`isNumber()`), it writes a raw byte for the number type (`ValueType::VAL_NUMBER`) followed by the actual numeric value.

4. **String Values**: When serializing strings (`isString()`), it starts by writing a raw byte for the string type (`ValueType::VAL_STRING`). After that, it calls the `writeString` function to handle the conversion and writing of the string data to the output stream.

5. **Array Values**: Arrays (`isArray()`) are serialized by writing a raw byte for the array type (`ValueType::VAL_ARRAY`). Following this, it writes the size of the array as a 32-bit unsigned integer. Each element in the array is then recursively serialized using the `writeValue` function.

6. **Function Values**: Functions (`isFunction()`) are serialized as closures (`ValueType::VAL_CLOSURE`). It writes a raw byte for the closure type and then calls the `writeChunk` function to serialize the chunk associated with the function.

7. **Edge Cases**:
   - If the input value (`val`) is of an unsupported type, the function throws a runtime error indicating the inability to serialize the value.

## Interactions with Other Components

- **Output Stream (`out`)**: The `writeValue` function interacts with the output stream to write serialized data. It uses various helper functions like `writeRaw` and `writeString` to perform low-level operations on the stream.

- **Value Type Handling**: The function relies on the `ValueType` enumeration to identify and handle different types of values. These types include `VAL_NIL`, `VAL_BOOL`, `VAL_NUMBER`, `VAL_STRING`, `VAL_ARRAY`, and `VAL_CLOSURE`.

- **Helper Functions**: Several helper functions such as `writeRaw`, `writeString`, and `writeChunk` are used internally to manage the serialization process. These functions typically operate at a lower level, dealing with individual bytes or chunks of data.

Overall, the `writeValue` function is a fundamental part of the Quantum Language compiler's serialization mechanism, ensuring that all types of values are correctly converted and stored in a binary format. This capability is essential for maintaining data integrity and facilitating efficient communication within the compiler and its ecosystem.