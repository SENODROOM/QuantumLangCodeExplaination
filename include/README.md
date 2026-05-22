# QuantumLanguage Compiler - Error.h

## Overview

The `include/Error.h` header file is crucial for the QuantumLanguage compiler, focusing on error handling mechanisms. It defines custom exception classes that extend the standard library's `std::runtime_error`, providing additional information such as the error type and line number where the error occurred. This allows for more precise diagnostics and better user experience during the compilation and execution phases.

## Role in Compiler Pipeline

In the QuantumLanguage compiler pipeline, `Error.h` plays a vital role in managing errors throughout the compilation process. Errors can be detected at various stages, including parsing, semantic analysis, and code generation. By using these custom exception classes, the compiler can throw specific types of errors when issues arise, making it easier to identify and address problems. These exceptions are then caught and handled appropriately, either by reporting them to the user or by correcting them within the compiler itself.

## Key Design Decisions and Why

1. **Custom Exception Classes**: Extending `std::runtime_error` with specialized subclasses like `RuntimeError`, `TypeError`, etc., provides a clear and structured way to handle different types of errors. This makes the code more readable and maintainable, as each type of error has its own class with descriptive names.

2. **Line Number Information**: Including the line number in the error message helps developers quickly locate the source of the problem in their code. This is particularly useful during debugging and development phases.

3. **Color Coding**: The `Colors` namespace contains ANSI escape codes for text coloration, which can be used to visually distinguish between different types of error messages. For example, runtime errors might be displayed in red, while syntax errors could be yellow. This enhances the readability and effectiveness of error communication.

## Major Classes/Functions Overview

### QuantumError Class

- **Purpose**: Base class for all quantum-specific errors.
- **Attributes**:
  - `line`: The line number where the error occurred.
  - `kind`: A string indicating the type of error (e.g., "RuntimeError").
- **Constructor**: Initializes the base class with a message and optionally a line number and error type.

### RuntimeError Class

- **Purpose**: Represents runtime errors that occur during the execution of the program.
- **Constructor**: Calls the constructor of `QuantumError` with the type set to "RuntimeError".

### TypeError Class

- **Purpose**: Indicates type-related errors, such as mismatched data types.
- **Constructor**: Calls the constructor of `QuantumError` with the type set to "TypeError".

### NameError Class

- **Purpose**: Used for errors related to undefined variables or invalid identifiers.
- **Constructor**: Calls the constructor of `QuantumError` with the type set to "NameError".

### IndexError Class

- **Purpose**: Handles errors that occur when accessing elements outside the valid range of a sequence.
- **Constructor**: Calls the constructor of `QuantumError` with the type set to "IndexError".

### Colors Namespace

- **Purpose**: Provides ANSI escape codes for text coloration.
- **Contents**:
  - Various color constants (`RED`, `YELLOW`, `WHITE`, etc.) and formatting codes (`BOLD`, `RESET`).

## Tradeoffs

1. **Performance**: Using custom exception classes introduces some overhead compared to using basic strings or integers for error codes. However, this is generally negligible unless the compiler is running in a performance-critical environment.

2. **Complexity**: Adding more specific error types increases the complexity of the error handling system. While this can lead to more robust and informative error messages, it also requires more maintenance and testing.

3. **Readability**: Color coding error messages improves readability but may not be supported in all environments or terminals. Additionally, overuse of colors can clutter the output and make it harder to focus on important details.

Overall, the `include/Error.h` header file is designed to provide a comprehensive and user-friendly error handling mechanism for the QuantumLanguage compiler, balancing functionality, performance, and readability.