# QuantumLanguage Compiler - Error.h

## Overview

The `include/Error.h` header file is an essential part of the QuantumLanguage compiler, dedicated to defining custom error types and utilities for exception handling. These errors are designed to provide detailed information about issues encountered during compilation or execution, aiding developers in debugging and improving their code quality.

## Role in Compiler Pipeline

In the QuantumLanguage compiler's pipeline, the `Error.h` header plays a crucial role by facilitating the propagation of errors throughout the system. It ensures that errors are not only caught but also properly reported with relevant context, such as line numbers and error kinds.

### Key Design Decisions and Why

- **Custom Exception Classes**: The header defines several custom exception classes (`QuantumError`, `RuntimeError`, `TypeError`, `NameError`, `IndexError`). Each class inherits from `std::runtime_error` and includes additional attributes like `line` and `kind`. This design allows for more specific error categorization and easier identification of error sources.

- **Error Kind**: By including an error kind in each exception class, the compiler can differentiate between different types of errors. For example, distinguishing between a `RuntimeError` and a `TypeError` helps in providing targeted solutions and better error messages.

- **Line Number Information**: Providing line number information in exceptions aids developers in pinpointing exact locations where errors occur. This feature is particularly useful during development and debugging phases.

- **Color Coding**: The `Colors` namespace contains ANSI escape codes for various colors, which are used to color-code error messages in the console output. This makes it visually easier to distinguish between different types of errors and enhances user experience.

## Major Classes/Functions Overview

### QuantumError Class

- **Purpose**: Base class for all quantum-related errors.
- **Attributes**:
  - `line`: The line number where the error occurred.
  - `kind`: A string indicating the type of error.
- **Constructor**: Initializes the base class with a message, line number, and error kind.

### RuntimeError Class

- **Purpose**: Represents runtime errors that occur during the execution of a quantum program.
- **Constructor**: Calls the constructor of `QuantumError` with the appropriate error kind ("RuntimeError").

### TypeError Class

- **Purpose**: Indicates type-related errors, such as mismatched data types.
- **Constructor**: Calls the constructor of `QuantumError` with the appropriate error kind ("TypeError").

### NameError Class

- **Purpose**: Used when a variable or function name is not found.
- **Constructor**: Calls the constructor of `QuantumError` with the appropriate error kind ("NameError").

### IndexError Class

- **Purpose**: Denotes errors related to index access, such as accessing an out-of-bounds element in an array.
- **Constructor**: Calls the constructor of `QuantumError` with the appropriate error kind ("IndexError").

### Colors Namespace

- **Purpose**: Provides ANSI escape codes for coloring console output.
- **Contents**:
  - Various color constants (`RED`, `YELLOW`, `WHITE`, `CYAN`, `GREEN`, `BLUE`, `BOLD`, `RESET`, `MAGENTA`).

## Tradeoffs

- **Performance Overhead**: Using custom exception classes introduces some performance overhead compared to standard exceptions. However, this is generally negligible unless a large number of exceptions are thrown frequently.

- **Complexity**: Adding custom attributes and constructors increases the complexity of the error handling mechanism. While this adds flexibility, it may require more maintenance and testing.

- **Console Output Readability**: Color coding error messages improves readability in the console, especially for complex outputs. However, it might not be suitable for environments where colored text is not supported.

Overall, the `include/Error.h` header file is a vital component of the QuantumLanguage compiler, offering robust error handling mechanisms that enhance both the developer experience and the reliability of the compiled programs.