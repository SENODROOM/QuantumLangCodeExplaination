# QuantumLanguage Compiler - Error.h

## Overview

The `include/Error.h` header file is an essential part of the QuantumLanguage compiler, dedicated to defining custom error types and utilities for managing errors during compilation and execution. These error types extend standard C++ exception classes (`std::runtime_error`) and provide additional information such as the error type, message, and line number where the error occurred. The file also includes a namespace `Colors` with ANSI escape codes for console text formatting, enhancing the readability and visual appeal of error messages.

This header file plays a critical role in the compiler's error handling mechanism, allowing developers to catch and respond to various types of errors more effectively. It ensures that errors are not only informative but also visually distinct, making it easier to identify issues in large codebases.

## Key Design Decisions and Why

### Custom Exception Classes

- **QuantumError**: Base class for all custom errors, extending `std::runtime_error`. It includes additional members for error type and line number, providing more context about the error.
- **RuntimeError**, **TypeError**, **NameError**, **IndexError**: Derived classes from `QuantumError`, each representing a specific type of error commonly encountered during execution. This hierarchical structure makes it easy to handle different error types separately.

### Error Type Information

By including the error type (`kind`) in the base class, the compiler can differentiate between different types of errors at runtime. This is particularly useful for logging and debugging purposes, as it allows developers to quickly understand what went wrong without having to sift through extensive logs.

### Line Number Tracking

Tracking the line number where an error occurs provides valuable context for developers. It helps them pinpoint the exact location of the issue, facilitating quicker resolution and reducing the time spent tracing back errors.

### Console Text Formatting

The `Colors` namespace offers a set of ANSI escape codes for formatting console output. By using these codes, error messages can be displayed in bold red or other colors, making them stand out against normal text. This enhances the visibility of errors in the terminal, especially when dealing with long compilations or multiple error messages.

## Major Classes/Functions Overview

### QuantumError Class

- **Purpose**: Acts as the base class for all custom errors, providing common functionality and additional members for error type and line number.
- **Constructor**: Takes an error type (`kind`), an error message (`msg`), and an optional line number (`line`). Initializes the base class with the message and sets the error type and line number.

### RuntimeError Class

- **Purpose**: Represents runtime errors, which occur during the execution of the program.
- **Constructor**: Inherits from `QuantumError`, setting the error type to "RuntimeError".

### TypeError Class

- **Purpose**: Represents type-related errors, which occur when operations are performed on incompatible data types.
- **Constructor**: Inherits from `QuantumError`, setting the error type to "TypeError".

### NameError Class

- **Purpose**: Represents errors related to undefined names or variables.
- **Constructor**: Inherits from `QuantumError`, setting the error type to "NameError".

### IndexError Class

- **Purpose**: Represents errors related to invalid index access, typically in arrays or lists.
- **Constructor**: Inherits from `QuantumError`, setting the error type to "IndexError".

### Colors Namespace

- **Purpose**: Provides ANSI escape codes for console text formatting, enabling colorful and stylized output.
- **Members**: A collection of constants representing different colors and styles (e.g., RED, YELLOW, BOLD, RESET).

## Tradeoffs

- **Extensibility**: By creating a hierarchy of custom exception classes, the system becomes highly extensible. Adding new error types is straightforward and does not require significant changes to existing code.
- **Readability**: Using descriptive error types and line numbers improves the readability of error messages, making it easier for developers to understand and resolve issues.
- **Performance**: While adding additional metadata to exceptions may slightly impact performance, the benefits in terms of clarity and maintainability often outweigh this cost.
- **Complexity**: The introduction of custom exception classes adds some complexity to the codebase. However, this complexity is managed through clear naming conventions and well-defined interfaces.

Overall, the `include/Error.h` header file is a vital component of the QuantumLanguage compiler, offering robust error handling capabilities while maintaining simplicity and extensibility.