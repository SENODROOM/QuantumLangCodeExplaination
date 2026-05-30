# `readChunk` Function

## Purpose
The `readChunk` function is responsible for deserializing a binary data stream into a `Chunk` object. A `Chunk` represents a segment of code or data within a larger program or module in the Quantum Language compiler. This function reads various fields from the input data and populates the corresponding members of the `Chunk` object.

## Parameters
- `const char* data`: A pointer to the binary data stream that needs to be deserialized.
- `size_t& offset`: A reference to the current offset in the data stream. The offset is incremented as the function reads different fields from the data.

## Return Value
- Returns a shared pointer to a newly created `Chunk` object containing the deserialized data.

## How It Works
1. **Reading Chunk Name**:
   - The function starts by reading a string from the data stream using the `readString` function. This string is stored in the `name` member of the `Chunk` object.

2. **Reading Instructions**:
   - The next field is an unsigned 32-bit integer (`uint32_t`) representing the size of the instruction array. 
   - The function reserves space in the `chunk->code` vector based on this size.
   - It then iterates through each instruction, reading the operation type (`Op`), operand (`int32_t`), and line number (`int`). Each instruction is stored in a `Instruction` struct and added to the `chunk->code` vector.

3. **Reading Parameters**:
   - Another unsigned 32-bit integer specifies the number of parameters.
   - Space is reserved in the `chunk->params` vector.
   - For each parameter, a string is read from the data stream and added to the `chunk->params` vector.

4. **Reading Parameter Reference Flags**:
   - An unsigned 32-bit integer indicates the number of flags indicating whether each parameter is a reference.
   - Space is reserved in the `chunk->paramIsRef` vector.
   - For each flag, a boolean value is determined by checking if the raw byte is non-zero. This boolean value is then added to the `chunk->paramIsRef` vector.

5. **Reading Upvalue Count**:
   - The function reads an integer representing the count of upvalues used in the chunk. This value is stored in the `chunk->upvalueCount` member.

6. **Reading Constants**:
   - An unsigned 32-bit integer specifies the number of constants.
   - Space is reserved in the `chunk->constants` vector.
   - For each constant, the `readValue` function is called to deserialize the value from the data stream. The deserialized value is then added to the `chunk->constants` vector.

## Edge Cases
- If the data stream is shorter than expected at any point during deserialization, the function may throw exceptions or produce undefined behavior.
- Empty strings or arrays can be handled gracefully, but care must be taken to ensure that the `offset` is correctly updated even when no data is read.

## Interactions with Other Components
- The `readString`, `readRaw`, and `readValue` functions are assumed to be part of the same library or utility class, providing functionality for reading strings, raw data types, and values respectively.
- The `Chunk` class is likely defined elsewhere in the project, encapsulating the structure of a code or data segment.
- This function interacts with the rest of the Quantum Language compiler's serialization/deserialization framework, ensuring that all necessary data is correctly parsed and reconstructed into `Chunk` objects.