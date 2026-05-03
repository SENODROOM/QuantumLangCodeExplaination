# `writeValue`

The `writeValue` function is a crucial method in the Quantum Language compiler's serialization process. It is responsible for converting a given value (`val`) into a binary format and writing it to an output stream (`out`). This function ensures that all types of values can be serialized correctly, maintaining data integrity during the compilation and execution phases.

## Parameters/Return Value

- **Parameters**:
  - `out`: A reference to an output stream where the serialized value will be written.
  - `val`: The value to be serialized.

- **Return Value**: None. The function writes directly to the output stream and does not return any value.

## How It Works

The `writeValue` function operates based on the type of the input value (`val`). Here’s how it handles different types:

1. **Nil Values**:
   - If `val` is a nil value, it writes a raw byte representing `ValueType::VAL_NIL` to the output stream.

2. **Boolean Values**:
   - For boolean values, it first writes a raw byte indicating `ValueType::VAL_BOOL`.
   - Then, it writes another raw byte, either `1` or `0`, depending on whether the boolean value is `true` or `false`.

3. **Numeric Values**:
   - Numeric values are handled similarly to boolean values. The function starts by writing a raw byte indicating `ValueType::VAL_NUMBER`.
   - Next, it writes the numeric value itself using the appropriate binary representation.

4. **String Values**:
   - String values begin with a raw byte signifying `ValueType::VAL_STRING`.
   - Following this, the function calls `writeString` to handle the string serialization, which includes writing the length of the string followed by the actual characters.

5. **Array Values**:
   - Arrays start with a raw byte denoting `ValueType::VAL_ARRAY`.
   - The size of the array is then written as a 32-bit unsigned integer.
   - Each element in the array is recursively serialized by calling `writeValue` on each item.

6. **Function Values**:
   - Function values are serialized as closures, indicated by a raw byte of `ValueType::VAL_CLOSURE`.
   - The chunk associated with the function is written using the `writeChunk` method, which handles the binary representation of the code block.

7. **Edge Cases**:
   - If the input value is of an unsupported type, the function throws a runtime error indicating the inability to serialize the value.

## Interactions with Other Components

- **Serialization Manager**: The `writeValue` function is part of a broader serialization system managed by the Serialization Manager. This manager coordinates the serialization process across various parts of the compiler, ensuring consistency and efficiency.

- **Output Stream**: The function interacts directly with the output stream provided by the caller. This stream could be part of a file, memory buffer, or network connection, depending on the context of serialization.

- **Value Type Handling**: The function relies on the `ValueType` enumeration to identify and handle different types of values. This enumeration is likely defined elsewhere in the compiler to provide a standardized way of referring to value types.

In summary, the `writeValue` function plays a vital role in the Quantum Language compiler's serialization process by handling various types of values efficiently and ensuring their correct representation in binary format. Its robust implementation caters to different scenarios and interacts seamlessly with other critical components of the compiler.