# QuantumLanguage Compiler - Error.h

## Overview

The `include/Error.h` header file is essential to the QuantumLanguage compiler, providing a structured approach to error handling within the system. This file defines various custom exception classes derived from `std::runtime_error`, each tailored to handle specific types of errors encountered during compilation or program execution.

## Role in Compiler Pipeline

The `Error.h` file serves as a critical component of the compiler's error management system. During the compilation process, these custom exceptions help identify and report errors more effectively than standard exceptions. When an error occurs, the compiler can throw one of these exceptions, which are then caught and handled appropriately, ensuring that the user receives clear and actionable feedback about what went wrong and where.

## Key Design Decisions and Why

1. **Custom Exception Classes**: By creating custom exception classes (`QuantumError`, `RuntimeError`, `TypeError`, `NameError`, `IndexError`), the compiler can provide more precise error messages tailored to different types of issues. This enhances debugging and user experience by making it easier to understand the nature of the problem.
   
2. **Line Number Information**: Each error class includes a `line` member variable, allowing the compiler to report errors along with their corresponding line numbers in the source code. This is invaluable for pinpointing the exact location of the error, facilitating quick resolution.

3. **Color Coding**: The `Colors` namespace provides ANSI escape codes for color coding error messages. This makes error output visually distinct, improving readability and distinguishing between different types of errors at a glance.

## Major Classes/Functions Overview

- **QuantumError**: A base class for all custom QuantumLanguage exceptions. It inherits from `std::runtime_error` and adds a `line` number and an error `kind`.
  
  ```cpp
  class QuantumError : public std::runtime_error {
  public:
      int line;
      std::string kind;

      QuantumError(const std::string &kind, const std::string &msg, int line = -1);
  };
  ```

- **RuntimeError**: Derived from `QuantumError`, this class is used for runtime errors that occur during the execution of a compiled program.
  
  ```cpp
  class RuntimeError : public QuantumError {
  public:
      RuntimeError(const std::string &msg, int line = -1);
  };
  ```

- **TypeError**: Used for type-related errors, such as mismatched data types when performing operations.
  
  ```cpp
  class TypeError : public QuantumError {
  public:
      TypeError(const std::string &msg, int line = -1);
  };
  ```

- **NameError**: Thrown when a variable, function, or other entity is referenced but not defined.
  
  ```cpp
  class NameError : public QuantumError {
  public:
      NameError(const std::string &msg, int line = -1);
  };
  ```

- **IndexError**: Used for errors related to accessing elements outside the valid range of a sequence.
  
  ```cpp
  class IndexError : public QuantumError {
  public:
      IndexError(const std::string &msg, int line = -1);
  };
  ```

- **Colors Namespace**: Contains constants for ANSI escape codes, enabling color-coded error messages in the console.
  
  ```cpp
  namespace Colors {
      inline const char *RED = "\033[31m";
      inline const char *YELLOW = "\033[33m";
      // Other colors...
      inline const char *RESET = "\033[0m";
  }
  ```

## Tradeoffs

- **Performance Overhead**: Using custom exception classes instead of standard ones might introduce a slight performance overhead due to additional memory allocations and copying.
  
- **Complexity**: Adding custom exception classes increases the complexity of the codebase, requiring developers to be aware of the different error types and how they should be handled.

- **Readability vs. Precision**: While adding line number information and color coding improves the readability and precision of error messages, it might also clutter the console output, especially in large projects.

Overall, the `Error.h` file plays a vital role in enhancing the robustness and usability of the QuantumLanguage compiler by providing a comprehensive and flexible error handling mechanism.