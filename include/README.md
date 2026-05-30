# QuantumLanguage Compiler - Error.h

## Overview

The `include/Error.h` header file is a crucial component of the QuantumLanguage compiler, designed to handle various types of errors during the compilation process. By extending standard exception classes (`std::runtime_error`, `std::logic_error`), it provides a structured way to report errors with specific details such as error type, message, and line number. Additionally, it includes color codes for terminal output, enhancing the readability of error messages.

## Role in Compiler Pipeline

The `Error.h` file plays a vital role in the QuantumLanguage compiler's pipeline by providing mechanisms for error detection, reporting, and handling. It ensures that errors are not only caught but also communicated effectively to the user, allowing them to pinpoint issues within their source code accurately.

### Key Design Decisions and Why

1. **Extending Standard Exception Classes**: By inheriting from `std::runtime_error` and other relevant standard exception classes, the custom error classes provide a consistent interface for error handling while adding specific attributes like line numbers and error kinds.
   
   ```cpp
   class QuantumError : public std::runtime_error {
   public:
       int line;
       std::string kind;

       QuantumError(const std::string &kind, const std::string &msg, int line = -1)
           : std::runtime_error(msg), line(line), kind(kind) {}
   };
   ```

2. **Color Coding for Terminal Output**: Adding color codes allows for more visually distinct error messages, making it easier for developers to identify and address issues quickly.

   ```cpp
   namespace Colors {
       inline const char *RED = "\033[31m";
       inline const char *YELLOW = "\033[33m";
       // ... other colors
       inline const char *RESET = "\033[0m";
   }
   ```

## Major Classes/Functions Overview

### QuantumError Class

The base class for all quantum-specific errors. It inherits from `std::runtime_error` and adds additional attributes to store the error kind and line number.

```cpp
class QuantumError : public std::runtime_error {
public:
    int line;
    std::string kind;

    QuantumError(const std::string &kind, const std::string &msg, int line = -1);
};
```

### RuntimeError Class

A derived class representing runtime errors. These errors occur during the execution of the compiled code and are typically related to incorrect state or external factors.

```cpp
class RuntimeError : public QuantumError {
public:
    RuntimeError(const std::string &msg, int line = -1);
};
```

### TypeError Class

Represents errors related to type mismatches. These errors occur when operations are performed on variables of incompatible types.

```cpp
class TypeError : public QuantumError {
public:
    TypeError(const std::string &msg, int line = -1);
};
```

### NameError Class

Indicates errors related to undefined names or variable references. These errors occur when a name used in the code has not been declared or is out of scope.

```cpp
class NameError : public QuantumError {
public:
    NameError(const std::string &msg, int line = -1);
};
```

### IndexError Class

Represents errors related to invalid indices, such as accessing elements outside the bounds of arrays or lists.

```cpp
class IndexError : public QuantumError {
public:
    IndexError(const std::string &msg, int line = -1);
};
```

### Colors Namespace

This namespace contains constants for different color codes that can be used to style terminal output. The use of these colors enhances the visibility and distinguishability of error messages.

```cpp
namespace Colors {
    inline const char *RED = "\033[31m";
    inline const char *YELLOW = "\033[33m";
    // ... other colors
    inline const char *RESET = "\033[0m";
}
```

## Tradeoffs

- **Consistency vs. Customization**: While inheriting from standard exception classes ensures consistency across the compiler, it may limit customization options. However, this tradeoff is acceptable given the need for a unified error handling system.
  
- **Performance vs. Usability**: Using color codes for error messages improves usability but might slightly impact performance, particularly in environments where terminal coloring is disabled. Balancing these aspects requires careful consideration of the target audience and usage scenarios.

By leveraging these features, the `Error.h` file significantly contributes to the robustness and user-friendliness of the QuantumLanguage compiler, ensuring that developers can efficiently diagnose and fix issues in their code.