# QpmGzip.cpp

## Role in Compiler Pipeline

`QpmGzip.cpp` is a critical component of the Quantum Programming Machine (QPM) compiler that handles decompression of input data using the GZIP algorithm. This functionality is essential for processing compressed source files during compilation, ensuring efficient resource usage and faster build times.

## Key Design Decisions and Why

### Use of `zlib`
The choice of `zlib` as the compression library was driven by its widespread adoption, robustness, and performance. `zlib` provides a simple API for both compression and decompression, making it an ideal fit for integrating into the compiler pipeline.

### Auto-Detection of Headers
To support both GZIP and DEFLATE compressed streams, the code initializes the `z_stream` with `windowBits = 15 + 32`. This setting instructs `zlib` to automatically detect whether the incoming data is wrapped in a GZIP header or not, enhancing the flexibility and usability of the compiler.

## Major Classes/Functions Overview

### Class: None
This file does not contain any specific classes. Instead, it focuses on providing a utility function for decompressing GZIP-encoded data.

### Function: `gzipInflate`
- **Purpose**: Decompresses a GZIP-encoded string back to its original form.
- **Parameters**:
  - `input`: The GZIP-encoded data to be decompressed.
  - `output`: A reference to a string where the decompressed data will be stored.
  - `error`: A reference to a string where any error messages will be stored.
- **Return Value**: Returns `true` if the decompression is successful, otherwise returns `false`.
- **Process**:
  - Initializes a `z_stream` structure with appropriate settings (`windowBits = 15 + 32`).
  - Sets up the input buffer with the provided encoded data.
  - Iteratively processes the input data in chunks, appending the decompressed output to the `output` string.
  - Handles different states and errors during the decompression process, including checking for truncated or corrupt streams.

## Tradeoffs

### Memory Usage
Using a fixed-size chunk buffer (256 KB) balances memory efficiency and performance. Smaller buffers might lead to more frequent reallocations and copying, while larger buffers could waste memory for smaller inputs.

### Error Handling
The function provides basic error handling by storing error messages in the `error` parameter. However, this might not cover all possible edge cases, such as extremely large compressed streams that exceed system resources.

### Performance vs. Flexibility
The use of `zlib`'s automatic header detection enhances the compiler's flexibility but may introduce slight overhead compared to manually specifying the compression type. Balancing these factors ensures that the compiler can handle various types of compressed inputs efficiently.

Overall, `QpmGzip.cpp` plays a vital role in supporting compressed input files within the QPM compiler, offering a reliable and flexible solution for decompression tasks.