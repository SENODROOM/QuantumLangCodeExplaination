# `readChunk` Function

## Purpose
The `readChunk` function is designed to deserialize a binary data stream into a `Chunk` object. A `Chunk` represents a segment of code or data within a larger program or module in the Quantum Language compiler. This function reads various fields from the input data and populates the corresponding members of the `Chunk` object.

## Parameters/Return Value
- **Parameters**:
  - `const uint8_t* data`: A pointer to the binary data stream being deserialized.
  - `size_t& offset`: A reference to an offset variable that keeps track of the current position in the data stream as the function reads different fields.

- **Return Value**:
  - `std::shared_ptr<Chunk>`: The function returns a shared pointer to a newly created `Chunk` object containing all the deserialized data.

## How It Works
1. **Create a Chunk Object**: 
   ```cpp
   auto chunk = std::make_shared<Chunk>();
   ```
   A new `Chunk` object is allocated using `std::make_shared`. This object will hold all the deserialized information about the chunk.

2. **Read Chunk Name**:
   ```cpp
   chunk->name = readString(data, offset);
   ```
   The name of the chunk is read from the binary data using the `readString` function. This string identifies the chunk and can be used for debugging or organizing purposes.

3. **Read Code Section**:
   ```cpp
   uint32_t codeSize = readRaw<uint32_t>(data, offset);
   chunk->code.reserve(codeSize);
   for (uint32_t i = 0; i < codeSize; ++i) {
       Instruction instr;
       instr.op = readRaw<Op>(data, offset);
       instr.operand = readRaw<int32_t>(data, offset);
       instr.line = readRaw<int>(data, offset);
       chunk->code.push_back(instr);
   }
   ```
   The size of the code section is first read as a `uint32_t`. Then, space is reserved in the `chunk->code` vector to accommodate the expected number of instructions. Instructions are then read one by one from the data stream. Each instruction consists of an operation (`Op`), an operand (`int32_t`), and a line number (`int`). These are stored in the `chunk->code` vector.

4. **Read Parameter Section**:
   ```cpp
   uint32_t paramsSize = readRaw<uint32_t>(data, offset);
   chunk->params.reserve(paramsSize);
   for (uint32_t i = 0; i < paramsSize; ++i) {
       chunk->params.push_back(readString(data, offset));
   }
   ```
   Similar to the code section, the size of the parameter section is read as a `uint32_t`. Space is reserved in the `chunk->params` vector to store the parameters. Each parameter is a string, which is read from the data stream and added to the `chunk->params` vector.

5. **Read Parameter Reference Flags**:
   ```cpp
   uint32_t refSize = readRaw<uint32_t>(data, offset);
   chunk->paramIsRef.reserve(refSize);
   for (uint32_t i = 0; i < refSize; ++i) {
       chunk->paramIsRef.push_back(readRaw<uint8_t>(data, offset) != 0);
   }
   ```
   The size of the parameter reference flags section is read as a `uint32_t`. Space is reserved in the `chunk->paramIsRef` vector to store boolean values indicating whether each parameter is a reference. Each flag is a single byte, which is read from the data stream. If the byte is non-zero, the corresponding parameter is marked as a reference.

6. **Read Upvalue Count**:
   ```cpp
   chunk->upvalueCount = readRaw<int>(data, offset);
   ```
   The count of upvalues (external variables captured by closures) is read as an `int`. This value indicates how many external variables the chunk depends on.

7. **Read Constants Section**:
   ```cpp
   uint32_t constSize = readRaw<uint32_t>(data, offset);
   chunk->constants.reserve(constSize);
   for (uint32_t i = 0; i < constSize; ++i) {
       chunk->constants.push_back(readValue(data, offset));
   }
   ```
   Finally, the size of the constants section is read as a `uint32_t`. Space is reserved in the `chunk->constants` vector to store the constant values used by the chunk. Each constant is read from the data stream using the `readValue` function and added to the `chunk->constants` vector.

## Edge Cases
- **Empty Data Stream**: If the data stream is empty when `readChunk` is called, the function may not behave correctly as it attempts to read from memory beyond the end of the stream.
- **Incorrect Data Format**: If the data format does not match expectations (e.g., if the sizes of sections are incorrect or the types of data do not match), the function may fail to deserialize the data properly.
- **Memory Allocation Failure**: If memory allocation fails during the reservation of space in vectors, the function may throw exceptions or leave the program in an undefined state.

## Interactions with Other Components
The `readChunk` function interacts with several other components of the Quantum Language compiler:

- **Data Deserialization Functions**: It uses functions like `readString`, `readRaw`, and `readValue` to extract specific types of data from the binary stream. These functions handle the conversion between binary representations and higher