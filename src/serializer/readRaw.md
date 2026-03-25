# `readRaw`

## Purpose

The `readRaw` function is designed to deserialize raw binary data into a specific type `T`. It reads the data directly from a buffer and returns an instance of `T`.

## Parameters

- `data`: A reference to a `std::vector<char>` containing the serialized data.
- `offset`: A reference to a `size_t` indicating the current position in the `data` vector where reading should begin.

## Return Value

- Returns an instance of type `T` that was deserialized from the binary data at the specified `offset`.

## How It Works

1. **Bounds Checking**: The function first checks if there is enough data left in the `data` vector starting from the given `offset` to accommodate a value of type `T`. If not, it throws a `std::runtime_error` with the message "Unexpected end of file while deserializing". This ensures that the function does not attempt to read beyond the bounds of the provided data, which could lead to undefined behavior or security vulnerabilities.

2. **Memory Copying**: If the bounds check passes, the function uses `std::memcpy` to copy the bytes from the `data` vector starting at `offset` to the memory location of the variable `t`. This effectively deserializes the binary data into a value of type `T`.

3. **Offset Update**: After copying the data, the function increments the `offset` by the size of `T`. This updates the position within the `data` vector so that subsequent calls to `readRaw` can continue reading from the correct place.

4. **Return Deserialized Value**: Finally, the function returns the deserialized value `t`.

## Edge Cases

- **Empty Data Vector**: If the `data` vector is empty, attempting to read any data will result in an immediate error because there is no data available to deserialize.
  
- **Incorrect Offset**: If the `offset` is greater than or equal to the size of the `data` vector, the function will also throw an error, as it indicates that the reading has already reached or surpassed the end of the data.

- **Type Mismatch**: The function assumes that the data at the specified `offset` matches exactly the type `T`. If there is a mismatch between the expected type and the actual data, the behavior is undefined, and the application may crash or produce incorrect results.

## Interactions With Other Components

The `readRaw` function is typically used within a larger deserialization framework. It might be called by methods that handle more complex structures, such as arrays, nested objects, or custom serialization formats. These higher-level methods would manage the overall state of the deserialization process, including updating the `offset` and handling errors that arise during the deserialization of individual elements.

In summary, `readRaw` provides a basic building block for deserializing binary data into specific types, ensuring safety through bounds checking and using efficient memory operations to perform the conversion. Its use in a broader context allows for the deserialization of complex data structures.