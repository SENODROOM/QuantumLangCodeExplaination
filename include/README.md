# QuantumLanguage Compiler - Error.h

## Overview

The `include/Error.h` header file is an essential part of the QuantumLanguage compiler, focusing on error handling mechanisms. It defines several custom exception classes that extend the standard `std::runtime_error`, each tailored to specific error conditions encountered during compilation or execution. Additionally, it includes a namespace for terminal color codes, which can be used to format error messages for better visibility.

This file plays a critical role in ensuring robustness and user-friendly error reporting throughout the QuantumLanguage ecosystem. By providing clear, categorized exceptions, it helps developers quickly identify and address issues in their code.

## Role in Compiler Pipeline

The `Error.h` file is integral to the compiler's error management stage. During parsing, semantic analysis, and execution phases, these exception classes are thrown when errors occur. The compiler then catches these exceptions and reports them to the user, often with additional context such as the line number where the error was detected.

## Key Design Decisions and Why

1. **Custom Exception Classes**: Extending `std::runtime_error` allows for more granular control over error types. Each class (`RuntimeError`, `TypeError`, etc.) clearly identifies a specific category of error, making debugging easier.

2. **Line Number Information**: Including the line number in the exception constructor provides valuable context to users. They can immediately locate the problematic section of their code, speeding up the resolution process.

3. **Color Coding**: The `Colors` namespace offers a simple way to format error messages with different colors. This visual distinction can help users quickly differentiate between different types of errors, enhancing readability.

## Major Classes/Functions Overview

### QuantumError Class

- **Purpose**: Base class for all custom quantum language errors.
- **Attributes**:
  - `int line`: Line number where the error occurred.
  - `std::string kind`: Type of error (e.g., "RuntimeError").
- **Constructor**: Takes a message, an optional line number, and an error kind.

### RuntimeError Class

- **Purpose**: Thrown for runtime errors, typically occurring during program execution.
- **Constructor**: Inherits from `QuantumError` with the kind set to "RuntimeError".

### TypeError Class

- **Purpose**: Thrown for type-related errors, such as incorrect variable assignments or function argument mismatches.
- **Constructor**: Inherits from `QuantumError` with the kind set to "TypeError".

### NameError Class

- **Purpose**: Thrown for errors related to undefined names or variables.
- **Constructor**: Inherits from `QuantumError` with the kind set to "NameError".

### IndexError Class

- **Purpose**: Thrown for index-related errors, such as accessing out-of-bounds elements in arrays.
- **Constructor**: Inherits from `QuantumError` with the kind set to "IndexError".

### Colors Namespace

- **Purpose**: Provides ANSI escape codes for formatting text in terminal output.
- **Contents**:
  - Various color constants (`RED`, `YELLOW`, `WHITE`, etc.).
  - A constant for resetting text color (`RESET`).

## Tradeoffs

- **Performance**: Using custom exceptions adds overhead compared to using raw strings or other simpler error handling methods. However, this tradeoff ensures more informative and manageable error reporting.
  
- **Complexity**: Adding multiple exception classes increases the complexity of the error handling system. While this might seem daunting, it ultimately leads to clearer and more maintainable code.

- **Readability**: Color coding error messages enhances readability but may not be supported in all environments. Balancing aesthetics with practicality is crucial for maintaining usability across different platforms.

Overall, the `Error.h` file is designed to provide comprehensive and user-friendly error handling capabilities, balancing performance, complexity, and readability. Its role in the compiler pipeline is vital for diagnosing and resolving issues effectively.