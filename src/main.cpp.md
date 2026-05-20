# Quantum Language Compiler - Main Entry Point

## Overview

`src/main.cpp` serves as the central entry point for the Quantum Language compiler, overseeing the entire compilation process. It interacts with various components such as the lexer, parser, compiler, virtual machine (VM), disassembler, type checker, and error handler to transform source code into executable programs or bytecode files. Additionally, it handles command-line arguments and controls the execution flow based on the build mode defined during compilation.

## Key Design Decisions and Why

### Modularity and Separation of Concerns

The compiler is designed with a modular architecture, where each component has a specific responsibility:
- **Lexer**: Converts raw text into tokens.
- **Parser**: Constructs an abstract syntax tree (AST) from tokens.
- **Compiler**: Translates AST into intermediate representation (IR).
- **VM**: Executes IR directly or compiles it into native code.
- **Disassembler**: Converts IR back into human-readable assembly code.
- **Type Checker**: Ensures that types are used correctly throughout the program.
- **Error Handler**: Manages and reports errors encountered during compilation.

This separation allows for easier maintenance, testing, and scalability of the compiler.

### Command-Line Argument Handling

The compiler processes command-line arguments to determine its operation mode:
- `QUANTUM_MODE_COMPILER`: Compiles `.sa` files into `.exe` files and executes them.
- `QRUN_MODE`: Always interprets `.sa` files without bundling.
- `neither`: Generates standalone bundled executables like `hello.exe`.

Handling these modes ensures flexibility in how the compiler can be used depending on the user's needs.

### Embedded Bytecode Support

For certain build configurations, the compiler supports embedded bytecode within the executable. This feature is particularly useful for creating self-contained applications. The bytecode is appended to the PE (Portable Executable) image at the end, and `loadEmbeddedBytecode` function reads and deserializes it when necessary.

## Major Classes/Functions Overview

### `main()`
- **Purpose**: The primary function that initializes the compiler and orchestrates the compilation process.
- **Flow**:
  1. Parses command-line arguments.
  2. Initializes the appropriate components based on the build mode.
  3. Reads input source files.
  4. Invokes the lexer, parser, compiler, and VM in sequence.
  5. Handles any errors that occur during the process.

### `getExecutablePath()`
- **Purpose**: Retrieves the full path of the currently executing executable.
- **Usage**: Used to locate the embedded bytecode section within the executable.

### `loadEmbeddedBytecode(const std::string &exePath)`
- **Purpose**: Loads and deserializes embedded bytecode from the specified executable path.
- **Details**:
  1. Opens the executable file in binary read mode.
  2. Checks the file size to ensure it contains enough data for the embedded bytecode.
  3. Verifies the presence of the "QNTM_VM!" magic string at the end of the file.
  4. Reads the payload size and the actual payload data.
  5. Deserializes the payload using `Serializer::deserialize`.
- **Return Value**: A shared pointer to the loaded `Chunk`, which represents the compiled bytecode.

### `printBanner()`
- **Purpose**: Prints a colorful banner to the console when the compiler starts.
- **Details**:
  - Uses ANSI escape codes to set text color and style.
  - Displays ASCII art representing the Quantum Language logo.

## Tradeoffs

### Memory Usage vs. Execution Speed

- **Embedded Bytecode**: While embedding bytecode reduces the need for external files, it increases memory usage because the entire bytecode must be loaded into memory along with the executable.
- **Interpretation vs. Compilation**: Choosing between interpretation and compilation affects both memory usage and execution speed. Interpretation is slower but uses less memory, while compilation generates faster-executing native code but requires more memory during the compilation phase.

### Flexibility vs. Simplicity

- **Modular Architecture**: Provides greater flexibility by allowing individual components to be modified or replaced independently. However, it adds complexity due to increased interdependencies and the need for careful management of resources.
- **Single-File vs. Multi-File**: Bundling bytecode into a single executable simplifies distribution and deployment but limits the ability to update or debug individual components separately.

By carefully balancing these factors, the Quantum Language compiler aims to provide a powerful yet flexible tool for developing quantum applications.