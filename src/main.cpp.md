# Quantum Language Compiler - Main Entry Point

## Overview

`src/main.cpp` is the central component of the Quantum Language compiler, responsible for orchestrating the entire compilation process. This includes managing interactions with the lexer, parser, compiler, virtual machine (VM), disassembler, type checker, and error handler. It also handles command-line arguments and directs the execution flow based on the mode specified during compilation (compiler, interpreter, or standalone bundled executable).

## Key Design Decisions and Why

1. **Modular Architecture**: The code is divided into several modules (`Lexer`, `Parser`, `Compiler`, `VM`, `Disassembler`, `TypeChecker`, `Error`) to ensure that each part can be developed, tested, and maintained independently. This modularity enhances scalability and maintainability.

2. **Command-Line Argument Handling**: The program parses command-line arguments to determine the mode of operation (compile, interpret, or run a bundled executable). This flexibility allows users to choose how they want to execute their Quantum programs.

3. **Embedded Bytecode Support**: For the standalone bundled executable mode, the program supports embedding bytecode directly into the executable file. This feature simplifies deployment and eliminates the need for separate bytecode files.

4. **Windows-Specific Features**: The code includes Windows-specific features such as using the Win32 API for bundling and launching executables. This ensures compatibility with Windows environments.

5. **Error Handling**: Robust error handling mechanisms are implemented throughout the program to manage and report errors effectively. This helps in debugging and improving user experience.

## Major Classes/Functions Overview

### `main.cpp`

- **Purpose**: Contains the main function which serves as the entry point of the Quantum Language compiler.
  
- **Key Functions**:
  - `getExecutablePath()`: Retrieves the path of the currently executing executable.
  - `loadEmbeddedBytecode(const std::string &exePath)`: Loads embedded bytecode from the executable file.
  - `printBanner()`: Prints a banner to the console when the program starts.

### `Lexer.h`

- **Purpose**: Responsible for breaking down source code into tokens.

### `Parser.h`

- **Purpose**: Parses tokens into an abstract syntax tree (AST).

### `Compiler.h`

- **Purpose**: Converts the AST into bytecode.

### `VM.h`

- **Purpose**: Executes the bytecode.

### `Disassembler.h`

- **Purpose**: Disassembles bytecode back into human-readable instructions.

### `TypeChecker.h`

- **Purpose**: Ensures that the types used in the program are correct and consistent.

### `Error.h`

- **Purpose**: Handles and reports errors encountered during the compilation process.

### `Value.h`

- **Purpose**: Represents values in the Quantum language, including integers, floats, strings, and more.

### `Serializer.h`

- **Purpose**: Serializes and deserializes bytecode for storage and retrieval.

## Tradeoffs

1. **Complexity vs. Simplicity**: While the modular architecture enhances maintainability, it adds complexity to the overall system. However, this complexity is manageable through proper documentation and testing.

2. **Performance vs. Memory Usage**: Embedding bytecode directly into the executable file reduces memory usage but may increase the size of the executable. Balancing these factors is crucial for performance optimization.

3. **Cross-Platform Compatibility**: The inclusion of Windows-specific features limits cross-platform compatibility. However, since Quantum Language targets primarily Windows environments, this tradeoff is acceptable.

4. **Development Time vs. Feature Set**: Adding advanced features like embedded bytecode support increases development time. However, these features provide significant benefits to users, justifying the extra effort.

By carefully considering these tradeoffs, the Quantum Language compiler aims to provide a powerful and efficient tool for developing quantum applications while maintaining a balance between complexity and usability.