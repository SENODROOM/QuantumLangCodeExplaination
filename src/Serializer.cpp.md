# Serializer.cpp

## Role in Compiler Pipeline

The `Serializer.cpp` file is an essential component of the Quantum Language compiler's backend, responsible for serializing and deserializing quantum values. This functionality enables the storage, transmission, and sharing of intermediate results across different stages of the compilation process or between systems efficiently.

## Key Design Decisions and Why

### 1. **Enum Class for Value Types**
   - **Why**: Using an enum class (`ValueType`) provides a clear and type-safe way to represent different types of quantum values. It avoids magic numbers and makes the code more readable and maintainable.

### 2. **Template Functions for Raw Data Handling**
   - **Why**: Templates allow the `Serializer` class to handle various data types uniformly. By using `writeRaw` and `readRaw`, the implementation becomes generic and reduces redundancy.

### 3. **Custom Serialization for Strings**
   - **Why**: Strings require special handling because their lengths need to be serialized along with the actual characters. This ensures that strings can be correctly reconstructed during deserialization.

### 4. **Recursive Serialization for Arrays**
   - **Why**: Arrays contain other quantum values, so recursive serialization is necessary to ensure all elements are properly saved and restored.

### 5. **Handling Function Values**
   - **Why**: Functions (closures) are complex objects that include chunks of bytecode. Custom serialization for functions ensures that the entire function state is preserved during the serialization process.

## Major Classes/Functions Overview

### `Serializer`
- **Purpose**: Manages the serialization and deserialization of quantum values.
- **Key Methods**:
  - `writeRaw`: Writes raw binary data to the output vector.
  - `readRaw`: Reads raw binary data from the input vector.
  - `writeString`: Writes a string to the output vector, including its length.
  - `readString`: Reads a string from the input vector based on its length.
  - `writeValue`: Serializes a `QuantumValue` object into the output vector.
  - `readValue`: Deserializes a `QuantumValue` object from the input vector.

### `ValueType`
- **Purpose**: Enumerates the different types of quantum values that can be serialized.
- **Values**:
  - `VAL_NIL`: Represents a null value.
  - `VAL_BOOL`: Represents a boolean value.
  - `VAL_NUMBER`: Represents a numeric value.
  - `VAL_STRING`: Represents a string value.
  - `VAL_ARRAY`: Represents an array of quantum values.
  - `VAL_CLOSURE`: Represents a function closure.

## Tradeoffs

### Performance vs. Flexibility
- **Performance**: The use of templates and direct memory manipulation (`std::memcpy`) can lead to performance optimizations, but it also increases complexity.
- **Flexibility**: Supporting multiple data types and complex structures like arrays and closures requires more robust and flexible code, which might impact performance slightly.

### Space Efficiency vs. Time Complexity
- **Space Efficiency**: Storing the length of strings explicitly can increase space usage, but it simplifies the deserialization process.
- **Time Complexity**: Reading and writing raw data directly is faster than converting data to and from a string format, but it requires careful handling to avoid buffer overflows.

By carefully balancing these considerations, the `Serializer.cpp` file ensures efficient and reliable data handling throughout the Quantum Language compiler's pipeline.