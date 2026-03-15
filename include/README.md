# QuantumLanguage Compiler - Error.h

## Overview

The `include/Error.h` header file is an essential component of the QuantumLanguage compiler's error handling mechanism. It defines custom exception classes for various types of errors that can occur during the compilation and execution phases of the compiler. These classes extend the standard `std::runtime_error` and provide additional information such as the error type and the line number where the error occurred. The file also includes a namespace `Colors` containing ANSI escape codes for text coloring, which can be used to highlight error messages in the console output.

This header file plays a critical role in ensuring robust error reporting and facilitating easier debugging by providing clear and specific error information throughout the compiler's lifecycle.

## Design Decisions

### Custom Exception Classes

Key design decisions were made to create custom exception classes for different types of errors. This approach allows for more precise error identification and handling, making it easier to diagnose issues and respond appropriately. By extending `std::runtime_error`, these classes inherit all the functionality of the base exception class while adding their own unique attributes.

#### Tradeoffs

- **Increased Complexity**: Defining multiple custom exception classes adds complexity to the codebase.
- **Memory Overhead**: Each exception object carries additional data (e.g., line number), which may lead to increased memory usage.

### Error Type and Line Number

Including both the error type and the line number in the exception classes provides valuable context for developers. Knowing the exact type of error and its location helps in pinpointing the cause of the issue quickly.

#### Limitations

- **Line Numbers**: While line numbers are useful, they might not always be available or accurate, especially in cases of syntax errors or dynamically generated code.
- **Error Types**: Although error types help categorize issues, they might not cover every possible scenario, leading to potential gaps in error handling.

### Text Coloring

Using ANSI escape codes to color text in the console output enhances readability and visual distinction between different types of error messages. This feature is particularly useful during development and testing phases when debugging is frequent.

#### Limitations

- **Platform Compatibility**: Not all platforms support ANSI escape codes, which could limit the effectiveness of this feature on certain systems.
- **Readability**: Overuse of colored text might reduce overall readability, especially if not properly contrasted against the background.

## Documentation

### QuantumError

**Purpose**: Base class for all custom exceptions in the QuantumLanguage compiler. Provides common functionality for all error types.

**Behavior**: Inherits from `std::runtime_error` and adds two additional members: `line` and `kind`.

```cpp
class QuantumError : public std::runtime_error
{
public:
    int line; // Line number where the error occurred
    std::string kind; // Type of error

    QuantumError(const std::string &kind, const std::string &msg, int line = -1)
        : std::runtime_error(msg), line(line), kind(kind) {}
};
```

### RuntimeError

**Purpose**: Represents runtime errors that occur during the execution of the program.

**Behavior**: Inherits from `QuantumError` and sets the error kind to "RuntimeError".

```cpp
class RuntimeError : public QuantumError
{
public:
    RuntimeError(const std::string &msg, int line = -1)
        : QuantumError("RuntimeError", msg, line) {}
};
```

### TypeError

**Purpose**: Represents type-related errors that occur due to mismatched data types.

**Behavior**: Inherits from `QuantumError` and sets the error kind to "TypeError".

```cpp
class TypeError : public QuantumError
{
public:
    TypeError(const std::string &msg, int line = -1)
        : QuantumError("TypeError", msg, line) {}
};
```

### NameError

**Purpose**: Represents errors related to undefined names or variables.

**Behavior**: Inherits from `QuantumError` and sets the error kind to "NameError".

```cpp
class NameError : public QuantumError
{
public:
    NameError(const std::string &msg, int line = -1)
        : QuantumError("NameError", msg, line) {}
};
```

### IndexError

**Purpose**: Represents errors related to accessing invalid indices in arrays or collections.

**Behavior**: Inherits from `QuantumError` and sets the error kind to "IndexError".

```cpp
class IndexError : public QuantumError
{
public:
    IndexError(const std::string &msg, int line = -1)
        : QuantumError("IndexError", msg, line) {}
};
```

### Colors Namespace

**Purpose**: Contains ANSI escape codes for text coloring, enhancing the visual presentation of error messages.

**Behavior**: Provides constants for different colors and formatting options.

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

## Usage

To use these error classes, simply throw an instance of the appropriate class when an error occurs. For example:

```cpp
throw RuntimeError("Division by zero", 42);
```

When catching these exceptions, you can access the error message, line number, and kind using the respective member variables:

```cpp
try {
    // Some code that might throw