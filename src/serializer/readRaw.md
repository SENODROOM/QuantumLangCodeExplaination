# `readRaw`

## Purpose

The `readRaw` function is designed to deserialize raw binary data into a specific type `T`. It reads the data directly from a buffer and returns an instance of `T`. This function is crucial in the process of reconstructing objects from their serialized form, ensuring that the original object's state is accurately restored.

## Parameters

- `data`: A reference to a `std::vector<uint8_t>` containing the serialized binary data.
- `offset`: An integer representing the current position within the `data` vector where reading should begin.

## Return Value

- Returns an instance of type `T`, which has been reconstructed from the binary data starting at the specified `offset`.

## How It Works

The `readRaw` function operates by copying a block of memory from the `data` vector into a variable of type `T`. Here’s how it ensures this:

1. **Bounds Checking**: Before proceeding with the copy operation, the function checks if there is enough data left in the `data` vector to accommodate the size of `T`. If not, it throws a `std::runtime_error` indicating an unexpected end of file during deserialization. This prevents potential buffer overflows or undefined behavior.

2. **Memory Copying**: The function uses `std::memcpy` to copy `sizeof(T)` bytes from the `data` vector, starting at the position indicated by `offset`, into the memory location of the variable `t`.

3. **Offset Update**: After successfully copying the data, the function updates the `offset` by adding `sizeof(T)`. This ensures that subsequent calls to `readRaw` will start reading from the correct position in the `data` vector.

4. **Return Instance**: Finally, the function returns the reconstructed instance of `T`.

## Edge Cases

- **Empty Data Vector**: If the `data` vector is empty, attempting to read any data will result in an immediate error due to the bounds check.
  
- **Insufficient Data**: If the remaining data in the `data` vector is less than `sizeof(T)`, the function will throw an exception. This handles scenarios where the serialized data is incomplete or corrupted.

- **Negative Offset**: Passing a negative `offset` will also lead to an error because the function expects a valid index within the `data` vector.

## Interactions with Other Components

The `readRaw` function interacts closely with the serialization framework, particularly with classes that implement serialization logic. These classes typically use `readRaw` to reconstruct their member variables from the serialized binary data. For example, when deserializing a complex object, each member might be read using `readRaw`, and then these members are used to initialize the object.

In summary, `readRaw` is a fundamental utility for deserializing binary data back into typed objects, providing both safety through bounds checking and efficiency through direct memory manipulation. Its interaction with other parts of the serialization system ensures that objects can be fully reconstructed from their serialized forms.