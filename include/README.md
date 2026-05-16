# QuantumLanguage Compiler - Error.h

## Overview

The `include/Error.h` header file is a crucial component of the QuantumLanguage compiler, designed to handle various types of errors encountered during compilation and execution. This file defines custom error classes that extend standard exception handling mechanisms, providing additional context such as error type and line number.

## Role in Compiler Pipeline

In the QuantumLanguage compiler pipeline, `Error.h` plays a vital role by enabling robust error management throughout the compilation process. Errors are detected at different stages, including parsing, semantic analysis, and code generation. By using these custom error classes, the compiler can provide more informative and actionable error messages to users, facilitating easier debugging and maintenance.

## Key Design Decisions and Why

### Custom Exception Classes

- **QuantumError**: Base class for all quantum-related errors, inheriting from `std::runtime_error`. It includes additional attributes like `line` and `kind`, which help in pinpointing the exact location and nature of the error.
  
- **RuntimeError, TypeError, NameError, IndexError**: Derived classes representing specific types of errors commonly encountered during runtime or semantic analysis. These classes simplify error categorization and provide clear feedback to developers.

### Error Context

Adding the `line` attribute to the base `QuantumError` class allows the compiler to report errors along with their occurrence line in the source code. This contextual information is invaluable for diagnosing issues quickly and accurately.

### Color Coding for User Feedback

The `Colors` namespace contains ANSI escape codes for color coding error messages. This feature enhances user experience by visually distinguishing between different types of errors, making it easier to identify and address them.

## Major Classes/Functions Overview

### QuantumError Class

- **Purpose**: Acts as a base class for all quantum-specific errors.
- **Attributes**:
  - `int line`: Line number where the error occurred.
  - `std::string kind`: Type of error (e.g., "RuntimeError", "TypeError").
- **Constructor**: Initializes the error message, line number, and kind.

### RuntimeError Class

- **Purpose**: Represents errors that occur during runtime.
- **Constructor**: Calls the base `QuantumError` constructor with the kind set to "RuntimeError".

### TypeError Class

- **Purpose**: Represents errors related to type mismatches.
- **Constructor**: Calls the base `QuantumError` constructor with the kind set to "TypeError".

### NameError Class

- **Purpose**: Represents errors related to undefined names or variables.
- **Constructor**: Calls the base `QuantumError` constructor with the kind set to "NameError".

### IndexError Class

- **Purpose**: Represents errors related to invalid array indices.
- **Constructor**: Calls the base `QuantumError` constructor with the kind set to "IndexError".

### Colors Namespace

- **Purpose**: Provides ANSI escape codes for color coding error messages.
- **Constants**:
  - `RED`, `YELLOW`, `WHITE`, `CYAN`, `GREEN`, `BLUE`, `BOLD`, `RESET`, `MAGENTA`: Escape codes for various colors and formatting options.

## Tradeoffs

### Additional Information vs. Complexity

By adding line numbers and error kinds to the base `QuantumError` class, the compiler gains more precise error reporting capabilities. However, this also increases the complexity of error handling code, requiring careful management of these additional attributes.

### Visual Clarity vs. Standard Output

Color coding error messages improves visual clarity and makes them stand out against normal text output. While this enhances usability, it may not be suitable for environments where colored output is not supported or preferred.

### Performance Overhead

Using ANSI escape codes for color coding introduces a minor performance overhead due to the need to format strings before outputting them. In most cases, this impact is negligible but should be considered when optimizing critical sections of the compiler.

Overall, `Error.h` provides a well-designed framework for managing errors in the QuantumLanguage compiler, balancing precision, usability, and performance considerations.