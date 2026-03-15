# QuantumLanguage Compiler - Error.h

## Overview

The `include/Error.h` header file is an essential part of the QuantumLanguage compiler, designed to manage and categorize errors encountered during the compilation process. This file provides a structured approach to error handling, ensuring that all errors are clearly identified and reported with relevant context, such as the type of error, the message, and the line number where it occurred.

## Key Design Decisions

### Inheritance from Standard Exception Classes

The primary reason for inheriting the custom error classes (`QuantumError`, `RuntimeError`, `TypeError`, etc.) from standard exception classes (`std::runtime_error`) is to leverage the robust error handling mechanisms provided by C++. By using standard exceptions, we can ensure compatibility with existing error handling frameworks and libraries, making it easier to integrate and debug error reporting in the compiler.

### Line Number Tracking

Tracking the line number where an error occurs is crucial for providing meaningful feedback to the user. This allows developers to quickly locate and fix issues in their code. The decision to include a line number attribute in the base `QuantumError` class ensures that all specific error types carry this information, facilitating more accurate error localization.

### Custom Error Types

Creating distinct error types for different categories of errors (e.g., `RuntimeError`, `TypeError`, `NameError`, `IndexError`) helps in distinguishing between different kinds of issues that may arise during compilation. Each derived class serves a specific purpose, allowing for targeted error handling and more informative error messages.

## Documentation

### QuantumError Class

**Purpose:** Base class for all quantum-specific errors. It inherits from `std::runtime_error` and adds additional attributes to track the error type and line number.

**Behavior:** 
- Takes three parameters: `kind` (the type of error), `msg` (the error message), and `line` (the line number where the error occurred).
- Inherits the error message functionality from `std::runtime_error`.

**Trade-offs/Limitations:**
- Adds overhead to error object creation due to the additional attributes.
- Requires careful management of error types to avoid confusion.

### RuntimeError Class

**Purpose:** Represents runtime errors, which occur during the execution of the program rather than at compile time.

**Behavior:** 
- Inherits from `QuantumError`.
- Initializes the `kind` attribute to "RuntimeError".

**Trade-offs/Limitations:**
- Limited specificity compared to other error types, but sufficient for most runtime issues.

### TypeError Class

**Purpose:** Represents type-related errors, which occur when operations are performed on incompatible data types.

**Behavior:** 
- Inherits from `QuantumError`.
- Initializes the `kind` attribute to "TypeError".

**Trade-offs/Limitations:**
- Specificity is high, making it easy to identify type-related issues.
- May not cover all possible type errors comprehensively.

### NameError Class

**Purpose:** Represents errors related to variable or function names, such as undefined variables or incorrect name usage.

**Behavior:** 
- Inherits from `QuantumError`.
- Initializes the `kind` attribute to "NameError".

**Trade-offs/Limitations:**
- Useful for identifying naming mistakes, which are common programming errors.
- Does not cover errors related to scope or namespace resolution.

### IndexError Class

**Purpose:** Represents errors related to accessing elements outside the bounds of arrays or similar data structures.

**Behavior:** 
- Inherits from `QuantumError`.
- Initializes the `kind` attribute to "IndexError".

**Trade-offs/Limitations:**
- Specific to array-like data structures, which are fundamental in many programming languages.
- May not be applicable to other data structures without modification.

### Colors Namespace

**Purpose:** Provides ANSI escape codes for text coloring, enhancing the readability of error messages in the terminal.

**Behavior:** 
- Contains constants for various colors (red, yellow, white, cyan, green, blue, bold, reset, magenta).
- These constants can be used to format error messages for better visibility.

**Trade-offs/Limitations:**
- Limited to terminal-based output, which may not be suitable for all environments.
- Use of escape codes can vary across terminals, potentially leading to inconsistent formatting.

## Conclusion

The `include/Error.h` header file plays a vital role in the QuantumLanguage compiler by providing a structured and informative way to handle errors. Its design decisions, including inheritance from standard exception classes, tracking line numbers, and creating specific error types, contribute to a more robust and user-friendly error reporting system. While there are some trade-offs and limitations, such as potential overhead and environment-specific formatting issues, these are outweighed by the benefits of clear and actionable error messages.