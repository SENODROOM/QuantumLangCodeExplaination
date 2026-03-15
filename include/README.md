# QuantumLanguage Compiler - Error.h

## Overview

The `include/Error.h` header file is an essential component of the QuantumLanguage compiler, responsible for managing and reporting errors encountered during the compilation process. It provides a structured way to handle different types of errors such as runtime, type, name, index, and quantum-specific errors, along with utilities for formatting error messages in a colorful and informative manner.

This file plays a critical role in ensuring that the compiler can effectively communicate issues to the user, allowing them to debug their code more efficiently. By centralizing error handling logic, it helps maintain consistency across the compiler's various stages and components.

## Design Decisions

### Inheritance from `std::runtime_error`

Inheriting from `std::runtime_error` allows the custom error classes (`QuantumError`, `RuntimeError`, `TypeError`, etc.) to utilize the standard exception handling mechanisms provided by C++. This includes the ability to catch exceptions using a base class pointer and leveraging polymorphism to handle specific error types differently.

**Why:** Standard library exceptions provide a well-established framework for error handling, making it easier to integrate with existing tools and practices. The use of inheritance simplifies the management of error types and behaviors.

### Adding Line Information

Each error class includes a `line` member variable, which stores the line number where the error occurred. This information is crucial for pinpointing the exact location of the problem in the source code.

**Why:** Providing line numbers enhances the debugging experience by allowing developers to quickly locate and fix issues within their code. This feature is particularly valuable when dealing with complex programs where errors may not be immediately apparent.

### Colorful Output

The `Colors` namespace contains constants for ANSI escape codes, enabling the compiler to output error messages in different colors. For example, runtime errors might be displayed in red, while syntax errors could be highlighted in yellow.

**Why:** Visual cues help users quickly identify the type of error they are encountering. Using color coding makes error messages more readable and distinguishes between different categories of issues.

## Classes and Functions

### QuantumError

```cpp
class QuantumError : public std::runtime_error
{
public:
    int line;
    std::string kind;

    QuantumError(const std::string &kind, const std::string &msg, int line = -1)
        : std::runtime_error(msg), line(line), kind(kind) {}
};
```

**Purpose:** Base class for all custom QuantumLanguage errors. Provides a common structure for storing error kind, message, and line number.

**Behavior:** Accepts an error kind, message, and optional line number. Initializes the base class with the message and adds the additional error details.

### RuntimeError

```cpp
class RuntimeError : public QuantumError
{
public:
    RuntimeError(const std::string &msg, int line = -1)
        : QuantumError("RuntimeError", msg, line) {}
};
```

**Purpose:** Represents runtime errors that occur during program execution.

**Behavior:** Inherits from `QuantumError` and initializes it with the "RuntimeError" kind and the provided message and line number.

### TypeError

```cpp
class TypeError : public QuantumError
{
public:
    TypeError(const std::string &msg, int line = -1)
        : QuantumError("TypeError", msg, line) {}
};
```

**Purpose:** Represents errors related to incorrect data types being used in operations.

**Behavior:** Inherits from `QuantumError` and initializes it with the "TypeError" kind and the provided message and line number.

### NameError

```cpp
class NameError : public QuantumError
{
public:
    NameError(const std::string &msg, int line = -1)
        : QuantumError("NameError", msg, line) {}
};
```

**Purpose:** Represents errors related to undefined names or variables.

**Behavior:** Inherits from `QuantumError` and initializes it with the "NameError" kind and the provided message and line number.

### IndexError

```cpp
class IndexError : public QuantumError
{
public:
    IndexError(const std::string &msg, int line = -1)
        : QuantumError("IndexError", msg, line) {}
};
```

**Purpose:** Represents errors related to invalid array or dictionary indices.

**Behavior:** Inherits from `QuantumError` and initializes it with the "IndexError" kind and the provided message and line number.

### Colors Namespace

```cpp
namespace Colors
{
    inline const char *RED = "\033[31m";
    inline const char *YELLOW = "\033[33m";
    inline const char *WHITE = "\033[37m";
    inline const char *CYAN = "\033[36m";
    inline const char *GREEN = "\033[32m";
    inline const char *BLUE = "\033[34m";
    inline const char *BOLD = "\033[1m";
    inline const char *RESET = "\033[0m";
    inline const char *MAGENTA = "\033[35m";
}
```

**Purpose:** Contains constants for ANSI escape codes to enable colorful output of error messages.

**Behavior:** Provides predefined escape codes for different colors and styles, which can be easily integrated into error message formatting.

## Tradeoffs and Limitations

- **Performance Overhead:** Using colored output via ANSI escape codes introduces a minor performance overhead, especially on systems without terminal support.
- **Complexity:** While providing a clear and informative interface for error reporting, the introduction of multiple error classes and color codes increases the complexity of the codebase.
- **Portability:** The use of ANSI escape codes limits the portability of the compiler to environments that do not support these codes.

Despite these limitations, the benefits of