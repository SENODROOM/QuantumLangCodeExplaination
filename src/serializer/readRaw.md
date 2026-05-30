# `readRaw`

## Purpose

The `readRaw` function is designed to deserialize raw binary data into a specific type `T`. It reads the data directly from a buffer and returns an instance of `T`. This function is crucial in the process of reconstructing objects from their serialized form, ensuring that the original object's state is accurately restored.

## Parameters

- `data`: A constant reference to a `std::vector<uint8_t>` representing the buffer containing the serialized data.
- `offset`: A reference to a `size_t` indicating the current position within the buffer. After reading the data, the offset is incremented by the size of `T`.

## Return Value

- Returns an instance of type `T`, which has been deserialized from the buffer starting at the specified offset.

## Edge Cases

1. **Buffer Overflow**: If the sum of the current offset and the size of `T` exceeds the total size of the buffer (`data.size()`), the function throws a `std::runtime_error` with the message "Unexpected end of file while deserializing". This ensures that the function does not attempt to read beyond the bounds of the buffer, preventing undefined behavior or crashes.
2. **Empty Buffer**: If the buffer is empty (`data.size() == 0`), attempting to read any data will result in a buffer overflow error because `offset + sizeof(T)` will be greater than `0`.
3. **Zero-Sized Type**: If the type `T` has zero size, the function will still increment the offset by `sizeof(T)`, effectively skipping over zero bytes in the buffer. However, since there is nothing to copy, the returned object will be default-initialized.

## Interactions with Other Components

The `readRaw` function interacts closely with the serialization system, particularly with the `Serializer` class. The `Serializer` class uses `readRaw` to deserialize various types of data stored in its internal buffer. For example, when reconstructing an object, the `Serializer` might call `readRaw` multiple times to deserialize different member variables of the object.

Here is a simplified code snippet demonstrating how `readRaw` might be used within the `Serializer` class:

```cpp
class Serializer {
public:
    // Constructor initializes the buffer and offset
    Serializer(const std::vector<uint8_t>& buffer)
        : data(buffer), offset(0) {}

    // Example method using readRaw to deserialize an integer
    int deserializeInt() {
        return readRaw<int>();
    }

private:
    std::vector<uint8_t> data; // Buffer containing serialized data
    size_t offset;             // Current position within the buffer

    template <typename T>
    T readRaw() {
        if (offset + sizeof(T) > data.size()) {
            throw std::runtime_error("Unexpected end of file while deserializing");
        }
        T t;
        std::memcpy(&t, data.data() + offset, sizeof(T));
        offset += sizeof(T);
        return t;
    }
};
```

In this example, the `deserializeInt` method calls `readRaw<int>()` to read an integer from the buffer. The `readRaw` function ensures that the read operation does not exceed the buffer's boundaries, thus maintaining the integrity of the serialization process.