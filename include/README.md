# QuantumLanguage Compiler - Error.h

## Overview

The `include/Error.h` header file is an essential part of the QuantumLanguage compiler, designed to manage and categorize errors encountered during compilation and runtime. This file provides a structured approach to error handling, ensuring that errors are not only thrown but also associated with specific details such as error type, message, and line number. The primary role of this file is to facilitate the reporting of meaningful error messages to the user, thereby improving the debugging experience.

## Key Design Decisions

### Inheritance from Standard Exceptions

The decision to inherit custom error classes (`QuantumError`, `RuntimeError`, `TypeError`, etc.) from `std::runtime_error` was made to leverage the robust exception handling mechanisms provided by C++. This inheritance ensures that all custom exceptions can be caught using a catch block for `std::exception`, making it easier to handle different types of errors uniformly across the compiler.

### Adding Line Number Information

Including the line number information in the error class was a critical design choice to enhance the accuracy of error reporting. By associating each error with a specific line number, developers can quickly locate the source of the issue in their code, significantly speeding up the debugging process.

### Custom Color Codes for Error Messages

The introduction of color codes through the `Colors` namespace was another deliberate design decision aimed at making error messages more visually distinct and easier to identify. Different colors can be used to highlight different types of errors, such as syntax errors in red, runtime errors in yellow, and so on. This visual distinction helps users quickly understand the nature of the error without having to read the entire message.

## Documentation of Major Classes/Functions

### QuantumError Class

**Purpose**: Base class for all quantum-specific errors. It extends `std::runtime_error` to provide additional context about the error.

**Behavior**: 
- Inherits from `std::runtime_error` to utilize standard exception handling.
- Adds two attributes: `line` (the line number where the error occurred) and `kind` (a string describing the type of error).

**Usage**: 
```cpp
throw QuantumError("SyntaxError", "Unexpected token '}'", 10);
```

### RuntimeError Class

**Purpose**: Represents errors that occur during runtime execution of the program.

**Behavior**: 
- Inherits from `QuantumError` and sets the `kind` attribute to `"RuntimeError"`.

**Usage**: 
```cpp
throw RuntimeError("Division by zero attempted");
```

### TypeError Class

**Purpose**: Indicates errors related to incorrect type usage in the program.

**Behavior**: 
- Inherits from `QuantumError` and sets the `kind` attribute to `"TypeError"`.

**Usage**: 
```cpp
throw TypeError("Cannot add integer and string");
```

### NameError Class

**Purpose**: Used when a variable or function name is not found.

**Behavior**: 
- Inherits from `QuantumError` and sets the `kind` attribute to `"NameError"`.

**Usage**: 
```cpp
throw NameError("Variable 'x' is not defined");
```

### IndexError Class

**Purpose**: Thrown when an index is out of bounds for an array or dictionary.

**Behavior**: 
- Inherits from `QuantumError` and sets the `kind` attribute to `"IndexError"`.

**Usage**: 
```cpp
throw IndexError("Attempted to access index 5 of an array with length 3");
```

### Colors Namespace

**Purpose**: Provides a set of predefined ANSI escape sequences for text coloring.

**Behavior**: 
- Contains constants representing different colors and formatting options (e.g., bold).
- Each constant returns a string that can be prepended to text to change its appearance.

**Usage**: 
```cpp
std::cout << Colors::RED << "This is a red error message" << Colors::RESET << std::endl;
```

## Tradeoffs/Limitations

### Overhead of Exception Handling

While exception handling is powerful, it comes with performance overhead. Using exceptions for control flow can lead to slower execution times compared to traditional if-else constructs. However, given the importance of accurate error reporting, this tradeoff was deemed necessary.

### Limited Use of Colors in Non-Terminal Environments

ANSI escape sequences for text coloring work well in terminal environments but may not render correctly in other contexts, such as certain IDEs or web-based terminals. This limitation means that while error messages are visually appealing in terminals, they might appear less informative in other environments.

### Static Nature of Color Codes

The use of static color codes within the `Colors` namespace implies that all error messages will follow the same styling rules. While this simplifies implementation, it may restrict the flexibility of error message presentation in complex scenarios.

## Conclusion

The `include/Error.h` header file plays a vital role in the QuantumLanguage compiler by providing a structured and informative way to handle errors. Through inheritance from standard exceptions, line number association, and custom color coding, this file enhances the debugging experience. Despite some limitations, such as potential performance impacts and restricted color support, the benefits of improved error reporting make it an indispensable part of the compiler's architecture.