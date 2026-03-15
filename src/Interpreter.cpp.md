# Interpreter.cpp

## Overview

`Interpreter.cpp` is a crucial component of the Quantum Language compiler, responsible for interpreting and executing quantum programs. This file contains the core logic of the interpreter, processing quantum instructions and managing the execution environment.

## Role in Compiler Pipeline

The `Interpreter` operates during the execution phase of the compiler pipeline. It accepts parsed quantum programs as input and executes them based on the language's semantics. The interpreter handles the evaluation of expressions, management of quantum states, and execution of control flow constructs such as loops and conditionals.

## Key Design Decisions

### Token Buffering

A static token buffer (`s_cinBuffer`) is used to manage leftover tokens when reading input using `std::cin`. This decision ensures that multiple `std::cin >>` operations can be performed without losing data between inputs, allowing for correct parsing of multi-token inputs.

**Why:** To maintain state across multiple `std::cin >>` operations, preventing data loss and ensuring accurate tokenization.

### Error Handling

Custom error handling mechanisms are implemented through the `TypeError` exception class. These exceptions provide clear and informative error messages, aiding in debugging and user experience.

**Why:** Custom exceptions offer more context-specific error information compared to standard library exceptions, enhancing the robustness and usability of the interpreter.

### Format Engine

A shared format engine (`applyFormat`) supports various formatting options similar to those found in C-style functions like `printf()`, `format()`, and `sprintf()`. This decision allows for consistent formatting across different output functions, reducing redundancy and improving maintainability.

**Why:** A unified format engine simplifies the implementation and usage of formatted output, making it easier to read and debug.

## Classes and Functions

### Interpreter Class

#### Purpose
The `Interpreter` class is the primary class responsible for interpreting and executing quantum programs.

#### Behaviour
- Parses quantum instructions from the input program.
- Manages quantum states and resources.
- Executes quantum operations and control flow constructs.
- Handles errors and exceptions during execution.

### applyFormat Function

#### Purpose
The `applyFormat` function applies formatting rules to strings, supporting various format specifiers and flags.

#### Behaviour
- Takes a format string and a vector of arguments.
- Processes the format string, replacing placeholders with corresponding argument values.
- Supports integer, floating-point, character, string, and hexadecimal formats.
- Handles alignment, padding, and precision flags.

### Helper Functions

- `toNum`: Converts a `QuantumValue` to a number, throwing an exception if the value is not numeric.
- `toInt`: Converts a `QuantumValue` to an integer, utilizing `toNum`.

## Tradeoffs and Limitations

- **Thread Safety:** The current implementation is not thread-safe, which may limit its scalability in concurrent environments.
- **Performance:** While the format engine provides flexibility, it may introduce performance overhead compared to optimized built-in functions.
- **Complexity:** The introduction of custom error handling and formatting mechanisms increases the complexity of the codebase.

## Usage

To use the `Interpreter`, include the necessary header files and create an instance of the `Interpreter` class. Pass the parsed quantum program to the `interpret()` method to execute it.

```cpp
#include "Interpreter.h"

int main()
{
    Parser parser;
    QuantumProgram program = parser.parse("your_quantum_program_here");
    
    Interpreter interpreter;
    try
    {
        interpreter.interpret(program);
    }
    catch (const TypeError &e)
    {
        std::cerr << "Error: " << e.what() << std::endl;
    }
    return 0;
}
```

This README.md provides an overview of the `Interpreter.cpp` file, explaining its role in the compiler pipeline, key design decisions, and documenting major classes and functions. It also highlights potential tradeoffs and limitations, ensuring transparency and informed use of the interpreter.