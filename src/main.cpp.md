# Quantum Language Compiler - Main Entry Point

## Overview

`src/main.cpp` is the central entry point for the Quantum Language compiler, responsible for orchestrating the entire compilation process. It interfaces with components like the lexer, parser, compiler, virtual machine (VM), disassembler, type checker, and error handler to convert source code into either executable programs or bytecode files. This file plays a crucial role in managing command-line arguments, handling input/output operations, and coordinating the flow of data through the compiler's various stages.

### Key Design Decisions and Why

The design of `main.cpp` involves several critical choices that ensure the flexibility and robustness of the compiler:

1. **Modular Architecture**: By dividing the functionality into distinct modules (`Lexer`, `Parser`, `Compiler`, `VM`, `Disassembler`, `TypeChecker`, `Error`), `main.cpp` facilitates easier maintenance and scalability. Each module can be developed, tested, and debugged independently.

2. **Command-Line Argument Handling**: The program uses `argc` and `argv` to parse command-line arguments, allowing users to specify options such as input file paths, output formats, and debugging levels. This approach ensures that the compiler can be used flexibly in different environments and configurations.

3. **Conditional Compilation**: Based on build definitions set by `CMakeLists.txt`, the program determines its mode of operation. For example:
   - `QUANTUM_MODE_COMPILER`: Compiles `.sa` files into `.exe` files and then executes them.
   - `QRUN_MODE`: Always interprets the source code without bundling it.
   - `neither`: Produces a standalone bundled executable, suitable for distribution.

4. **Error Handling**: The program includes comprehensive error handling mechanisms to manage syntax errors, semantic errors, and runtime exceptions. These mechanisms help in providing clear and actionable error messages, improving the user experience.

5. **Performance Optimization**: To enhance performance, especially during the compilation of large files, `main.cpp` employs techniques such as caching intermediate results and parallel processing where applicable. This optimization ensures that the compiler can handle complex projects efficiently.

## Major Classes/Functions Overview

- **`getExecutablePath()`**:
  - **Description**: Retrieves the full path of the currently executing binary.
  - **Usage**: Used to locate embedded bytecode within the executable file.

- **`loadEmbeddedBytecode(const std::string &exePath)`**:
  - **Description**: Loads and deserializes embedded bytecode from the specified executable file.
  - **Usage**: Enables the execution of precompiled bytecode directly from the executable, bypassing the need for separate compilation steps.

- **`printBanner()`**:
  - **Description**: Prints a colorful banner to the console when the compiler starts.
  - **Usage**: Enhances the visual appeal and provides immediate feedback to the user about the compiler's status.

## Tradeoffs

While `main.cpp` offers a modular and efficient architecture, it also comes with some tradeoffs:

- **Complexity**: Managing multiple modules and their interactions increases the complexity of the codebase. However, this complexity is mitigated by thorough testing and documentation.

- **Memory Usage**: Loading and deserializing embedded bytecode requires additional memory resources. While this can impact performance, it allows for more compact and convenient deployment of precompiled applications.

- **Build Time**: Conditional compilation based on build definitions can increase build time slightly. However, this tradeoff is acceptable given the benefits of flexible and optimized modes of operation.

In conclusion, `src/main.cpp` is a vital component of the Quantum Language compiler, ensuring its modularity, efficiency, and adaptability across different usage scenarios. Its design decisions and functionalities provide a solid foundation for developing, testing, and deploying quantum applications.