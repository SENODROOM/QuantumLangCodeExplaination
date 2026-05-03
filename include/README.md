# QuantumLanguage Compiler - Error.h

## Overview

The `include/Error.h` header file is an essential part of the QuantumLanguage compiler, focusing on defining various error types that can occur during the compilation and execution phases. These errors help in identifying issues such as type mismatches, name conflicts, index out-of-bounds conditions, and more. The file also includes utility functions for formatting error messages with colors, enhancing readability and debugging experience.

## Role in Compiler Pipeline

The `Error.h` header plays a crucial role in the QuantumLanguage compiler's pipeline:

1. **Compilation Phase**: Errors are thrown when the compiler encounters syntax or semantic issues in the source code. For example, attempting to use a variable before it has been declared will result in a `NameError`.
   
2. **Execution Phase**: During runtime, the virtual machine (VM) throws specific errors based on the encountered issues. A `TypeError` might be thrown if an operation is performed on incompatible data types.

These errors are caught and handled throughout the compiler pipeline, ensuring that any issues are identified and reported appropriately, thus improving the reliability and maintainability of the compiled code.

## Key Design Decisions and Why

### Hierarchical Error Classes

- **QuantumError**: Base class for all quantum-related errors, inheriting from `std::runtime_error`. It includes additional fields like `line` and `kind`, which provide context about the error location and type.
  
- **RuntimeError**, **TypeError**, **NameError**, **IndexError**: Derived classes representing specific types of errors. Each class provides a clear and concise description of the error, making it easier to understand and handle.

### Color-Coded Error Messages

- The `Colors` namespace contains constants for different color codes, allowing the compiler to format error messages with colors. This makes it visually easier to distinguish between different types of errors, especially in large codebases.

- Using ANSI escape sequences (`\033[...m`) for color coding ensures compatibility across different terminal environments.

## Major Classes/Functions Overview

### QuantumError Class

- **Purpose**: Acts as a base class for all quantum-related errors.
- **Fields**:
  - `int line`: Line number where the error occurred.
  - `std::string kind`: Type of the error.
- **Constructor**: Initializes the error message, line number, and kind.

### Derived Error Classes

- **RuntimeError**: Thrown for errors related to runtime behavior.
- **TypeError**: Thrown for type-related errors.
- **NameError**: Thrown for errors related to variable names.
- **IndexError**: Thrown for errors related to accessing invalid indices.

### Colors Namespace

- **Constants**: Provides predefined color codes for formatting error messages.
- **Usage**: Used within the compiler to output colored error messages in the terminal.

## Tradeoffs

- **Readability vs. Performance**: While using ANSI escape sequences for color coding enhances the readability of error messages, it may have minor performance implications due to the overhead of string manipulation.

- **Compatibility vs. Customization**: The use of standard ANSI escape sequences ensures broad compatibility across terminals, but it limits customization options compared to other methods of text formatting.

Overall, the `Error.h` header file is designed to provide a robust and user-friendly error handling mechanism, aiding developers in quickly identifying and resolving issues in their QuantumLanguage code.