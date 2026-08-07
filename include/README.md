# QuantumLanguage Compiler - Error.h

## Overview

The `include/Error.h` header file is an essential component of the QuantumLanguage compiler. This file defines custom exception classes that extend the standard library's `std::runtime_error`. These exceptions are used to handle various error conditions during the compilation and execution phases of the compiler.

## Role in Compiler Pipeline

The `Error.h` file serves as a critical interface for propagating errors throughout the compiler pipeline. By using these custom exception classes, the compiler can provide more specific and informative error messages to the user, which aids in debugging and improving the overall quality of the code.

### Key Design Decisions and Why

- **Custom Exception Classes**: Extending `std::runtime_error` allows for additional information such as the line number where the error occurred and the type of error (e.g., RuntimeError, TypeError).
- **Namespace for Colors**: Providing color codes within a namespace helps in formatting error messages in a visually appealing manner, making them easier to distinguish and understand.

## Major Classes/Functions Overview

### QuantumError Class

The base class for all quantum-specific errors. It inherits from `std::runtime_error` and adds two members:

- `int line`: Stores the line number where the error occurred.
- `std::string kind`: Specifies the type of error (e.g., "RuntimeError", "TypeError").

### RuntimeError Class

Derived from `QuantumError`, this class is specifically for runtime errors. It simplifies the creation of runtime error instances by setting the error kind to "RuntimeError".

### TypeError Class

Similar to `RuntimeError`, this class is derived from `QuantumError` and is used for type-related errors. It sets the error kind to "TypeError".

### NameError Class

This class is also derived from `QuantumError` and is used for errors related to undefined or misspelled names. It sets the error kind to "NameError".

### IndexError Class

Finally, this class is derived from `QuantumError` and is used for index-related errors, such as accessing out-of-bounds indices in arrays. It sets the error kind to "IndexError".

### Colors Namespace

The `Colors` namespace contains constants for different colors and formatting options. These are used to enhance the readability of error messages by providing visual cues.

## Tradeoffs

- **Additional Memory Usage**: Storing the line number and error kind in each exception object increases memory usage slightly but provides valuable context for debugging.
- **Complexity**: Introducing custom exception classes adds complexity to the codebase, requiring developers to be aware of the different types of errors and their associated kinds.
- **Performance Impact**: While the performance impact of using exceptions is generally minimal, it should be considered in high-performance scenarios where every microsecond counts.

By leveraging these custom exception classes and color-coded error messages, the QuantumLanguage compiler aims to provide a robust and user-friendly development experience, facilitating easier debugging and maintenance of the code.