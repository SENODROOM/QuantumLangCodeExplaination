# `writeValue`

The `writeValue` function is a critical method within the Quantum Language compiler's serialization process. Its primary objective is to convert a given value (`val`) into a binary format and then write it to an output stream (`out`). This functionality is vital for ensuring that values can be stored or transmitted efficiently and accurately between different parts of the compiler or even across different systems.

## What It Does

The `writeValue` function serializes various types of values such as nil, boolean, number, string, array, and closure. Each type is represented by a unique `ValueType` enum, which helps in identifying the type of data being serialized. The function writes the appropriate type identifier followed by the actual value data to the output stream.

## Why It Works This Way

This approach ensures that the serialized data is both compact and self-describing. By using a distinct type identifier for each value type, the deserialization process can easily identify the type of data and handle it appropriately. Additionally, writing raw binary data allows for efficient storage and transmission without the overhead of additional formatting.

## Parameters/Return Value

- **Parameters**:
  - `out`: A reference to an output stream where the serialized data will be written.
  - `val`: The value to be serialized.

- **Return Value**:
  - None. The function directly writes to the output stream.

## Edge Cases

1. **Nil Values**: When encountering a nil value, the function writes a special `ValueType::VAL_NIL` identifier to indicate the absence of any meaningful data.
   
2. **Boolean Values**: Boolean values are converted to a single byte (`0` for false, `1` for true). This ensures that boolean values take up minimal space while still being clearly distinguishable from other types.

3. **Number Values**: Numeric values are serialized directly as their binary representation. This approach assumes that the numeric type used in the compiler is compatible with the target system's numeric types.

4. **String Values**: String values are first written as a length-prefixed sequence of bytes. The length of the string is encoded as a 32-bit unsigned integer, followed by the actual string data. This ensures that strings of varying lengths can be handled correctly.

5. **Array Values**: Arrays are serialized as a length-prefixed sequence of elements. The length of the array is encoded as a 32-bit unsigned integer, followed by the serialized form of each element in the array. This approach ensures that arrays of any size can be serialized and deserialized accurately.

6. **Closure Values**: Closures are serialized as chunks of bytecode. The function identifier `ValueType::VAL_CLOSURE` is written first, followed by the chunk of bytecode representing the closure. This allows closures to be executed on the target system, assuming the same environment or interpreter is available.

7. **Unknown Types**: If the function encounters a value of an unknown type, it throws a runtime error indicating the inability to serialize the value. This prevents the serialization process from proceeding with incomplete or incorrect data.

## Interactions With Other Components

- **Serializer Class**: The `writeValue` function is part of the `Serializer` class, which manages the entire serialization process. It interacts with other methods like `writeRaw`, `writeString`, and `writeChunk` to perform low-level serialization tasks.

- **Value Class**: The `writeValue` function takes a `Value` object as its parameter. The `Value` class represents all possible values in the Quantum Language, including numbers, strings, arrays, and closures. The function uses methods like `isNil`, `isBool`, `asBool`, etc., provided by the `Value` class to determine the type of the value and extract its data.

- **Output Stream**: The function writes the serialized data to an output stream, which could be a file, a network socket, or any other writable resource. The output stream is passed as a reference to ensure efficient data handling and transfer.

In summary, the `writeValue` function plays a key role in the serialization process of the Quantum Language compiler. By handling various value types and ensuring correct data representation, it facilitates efficient storage and transmission of data between different parts of the compiler or across different systems.