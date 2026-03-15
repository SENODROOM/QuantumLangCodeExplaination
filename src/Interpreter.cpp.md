# Interpreter.cpp

## Overview

`Interpreter.cpp` is a critical component of the Quantum Language compiler, focusing on interpreting and executing quantum programs. This file encapsulates the core logic of the interpreter, which processes quantum instructions and manages the execution environment.

## Role in Compiler Pipeline

The `Interpreter` operates within the execution phase of the compiler pipeline. It takes in parsed quantum programs as input and executes them according to the language's rules. This ensures that the compiled code runs as intended, leveraging quantum computing principles effectively.

## Key Design Decisions

### Use of Static Variables

Static variables like `s_cinBuffer` are used to manage token buffering when reading input using `std::cin`. This decision was made to ensure that multiple `cin >>` operations can be performed without losing data between inputs. The choice of static storage duration allows the buffer to persist across function calls, facilitating seamless token management.

### Custom Error Handling

A custom error handling mechanism is implemented through the `Error` class. This approach was selected to provide more control over error reporting and recovery within the interpreter. By defining a dedicated error handling system, developers can tailor error messages to specific contexts, enhancing debugging and user experience.

### Support for Advanced Formatting

The `applyFormat` function supports advanced formatting similar to C-style functions (`printf`, `sprintf`). This decision was made to enable users to format output strings flexibly, aligning closely with standard practices in other programming languages. While this adds complexity to the implementation, it significantly improves the usability of formatted output in quantum applications.

## Documentation of Major Classes/Functions

### Class: `Interpreter`

**Purpose:** Manages the interpretation and execution of quantum programs.

**Behavior:** Parses quantum instructions and executes them in the specified order. Handles errors gracefully and provides feedback through the `Error` class.

### Function: `toNum`

**Purpose:** Converts a `QuantumValue` to a numeric type.

**Behavior:** Checks if the value is a number and returns it as a `double`. If not, throws a `TypeError`.

### Function: `toInt`

**Purpose:** Converts a `QuantumValue` to an integer type.

**Behavior:** Calls `toNum` to get the numeric representation and then casts it to a `long long`. This function simplifies the conversion process for integer values.

### Function: `applyFormat`

**Purpose:** Applies formatting to a string based on provided arguments.

**Behavior:** Supports various format specifiers such as `%d`, `%f`, `%s`, etc., and applies them to the corresponding arguments. Handles flags like `-` for left alignment, `+` for forced sign, and `0` for zero-padding.

## Tradeoffs/Limitations

- **Performance Over Memory Usage:** The use of static variables for token buffering may lead to increased memory usage, especially in scenarios where many threads are involved. However, it offers better performance compared to dynamically allocating buffers for each operation.
  
- **Complexity in Error Handling:** Implementing a custom error handling system increases the complexity of the codebase. While this enhances flexibility and control, it requires additional resources and time for maintenance.

- **Limited Support for Quantum Operations:** The current implementation focuses on basic quantum instruction processing. Extending support for more complex quantum operations would require significant architectural changes and additional testing.

## Conclusion

`Interpreter.cpp` plays a pivotal role in the Quantum Language compiler by interpreting and executing quantum programs efficiently. Through careful design decisions, including the use of static variables, custom error handling, and advanced formatting, the interpreter provides robust functionality tailored to the needs of quantum computing applications. Despite some inherent complexities and limitations, this implementation forms a solid foundation for further development and expansion.