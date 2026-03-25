# `readString` Function

## Overview

The `readString` function is part of the Quantum Language compiler's serialization module, located in the `src/Serializer.cpp` file. This function is responsible for reading a string from a serialized data buffer and updating the offset accordingly.

## What It Does

The primary purpose of `readString` is to extract a string from a byte array (`data`) starting at a specified offset (`offset`). The function reads the length of the string first as a 32-bit unsigned integer, then extracts the actual characters based on that length.

## Why It Works This Way

This implementation ensures that the string is correctly reconstructed from its serialized form without any corruption or out-of-bounds access. By reading the length first, the function can accurately determine how many bytes to allocate for the string, thus preventing potential issues related to buffer overflow.

## Parameters/Return Value

- **Parameters**:
  - `const std::vector<uint8_t>& data`: A constant reference to the byte array containing the serialized data.
  - `size_t& offset`: A reference to the current offset within the byte array. This parameter is updated to reflect the new position after reading the string.

- **Return Value**:
  - Returns a `std::string` object representing the extracted string.

## Edge Cases

1. **Empty String**: If the length of the string is zero, an empty string will be returned.
2. **Offset Out of Bounds**: If the calculated end offset exceeds the size of the data vector, a `std::runtime_error` is thrown to prevent buffer overflow.
3. **Non-ASCII Characters**: The function handles non-ASCII characters correctly by interpreting the bytes as a sequence of characters.

## Interactions With Other Components

- **`readRaw<T>` Function**: This function is used internally by `readString` to read a raw 32-bit unsigned integer from the data buffer. It is assumed to handle type-specific reading and error checking.
- **Error Handling**: When the calculated end offset exceeds the data buffer size, `readString` throws a `std::runtime_error`. This error handling mechanism is crucial for maintaining data integrity during deserialization processes.

## Example Usage

Here’s an example of how you might use the `readString` function:

```cpp
#include <iostream>
#include <vector>
#include "Serializer.h"

int main() {
    // Sample serialized data
    std::vector<uint8_t> data = {0x00, 0x00, 0x00, 0x05, 'H', 'e', 'l', 'l', 'o'};

    // Initialize offset
    size_t offset = 0;

    try {
        // Read the string from the serialized data
        std::string str = readString(data, offset);

        // Output the read string
        std::cout << "Read string: " << str << std::endl;
    } catch (const std::exception& e) {
        // Handle exceptions
        std::cerr << "Error: " << e.what() << std::endl;
    }

    return 0;
}
```

In this example, the `readString` function reads a string from a sample serialized data buffer and outputs it. The `offset` variable is updated automatically by the function to point to the next available byte after the string has been read.