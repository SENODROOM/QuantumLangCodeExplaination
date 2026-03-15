# Interpreter.cpp

## Overview

`Interpreter.cpp` is a crucial component of the Quantum Language compiler, responsible for interpreting and executing quantum programs. This file contains the implementation of the interpreter, which processes quantum instructions and manages the execution environment.

## Key Design Decisions

### Use of `std::istringstream` for Token Buffering

The decision to use `std::istringstream` for token buffering was made to handle the peculiarities of `std::cin >>`. Unlike `std::getline`, `std::cin >>` reads whitespace-separated tokens, which can lead to issues when dealing with multi-token inputs. By buffering leftover tokens, we ensure that subsequent `std::cin >>` operations read the correct values, even after a newline has been consumed.

**Why:** To maintain consistent behavior between `std::cin >>` and `std::getline`, especially when handling user input.

### Custom Error Handling

A custom error handling mechanism is implemented using the `Error` class. This allows for more precise control over error messages and their propagation throughout the compiler.

**Why:** To provide clear and informative error messages that help developers understand and fix issues in their quantum programs.

### Support for Various Formatting Options

The `applyFormat` function supports a wide range of formatting options, including `%d`, `%f`, `%s`, and more. These options allow for flexible output formatting, making it easier to generate human-readable results.

**Why:** To enable users to customize the output of their quantum programs according to their needs.

## Classes and Functions Documentation

### `class Interpreter`

**Purpose:** Manages the interpretation and execution of quantum programs.

**Behavior:** The interpreter reads quantum instructions from an input stream and executes them, updating the program state accordingly.

### `static double toNum(const QuantumValue &v, const std::string &ctx)`

**Purpose:** Converts a `QuantumValue` to a numeric type.

**Behavior:** If the `QuantumValue` is a number, it returns the corresponding numeric value. Otherwise, it throws a `TypeError`.

### `static long long toInt(const QuantumValue &v, const std::string &ctx)`

**Purpose:** Converts a `QuantumValue` to an integer type.

**Behavior:** Calls `toNum` to convert the `QuantumValue` to a numeric type and then casts it to a `long long`.

### `static std::string applyFormat(const std::string &fmt, const std::vector<QuantumValue> &args, size_t argStart = 1)`

**Purpose:** Applies a format string to a vector of arguments.

**Behavior:** Parses the format string and replaces placeholders with the corresponding arguments from the vector. Supports various formatting options such as integers, floating-point numbers, strings, and more.

## Tradeoffs and Limitations

- **Performance:** Using `std::istringstream` for token buffering may introduce some performance overhead compared to simpler methods.
- **Complexity:** Supporting a wide range of formatting options increases the complexity of the codebase.
- **Cross-Platform Compatibility:** The inclusion of Windows-specific cryptographic headers (`CryptGenRandom`) limits cross-platform compatibility, although this is not a concern for the target platform.

## Usage Example

Here's a simple example demonstrating how the `Interpreter` class might be used:

```cpp
#include "Interpreter.h"

int main()
{
    Interpreter interpreter;
    interpreter.execute("H 0");
    interpreter.execute("CNOT 0 1");
    interpreter.execute("M 0 1");

    return 0;
}
```

This example creates an `Interpreter` object and executes three quantum instructions: applying a Hadamard gate to qubit 0, creating a CNOT gate between qubits 0 and 1, and measuring both qubits.

## Conclusion

`Interpreter.cpp` plays a vital role in the Quantum Language compiler by providing the functionality to interpret and execute quantum programs. Its design decisions aim to balance flexibility, precision, and performance, ensuring that users can effectively develop and test their quantum applications. While there are some limitations, these are outweighed by the benefits provided by the interpreter's capabilities.