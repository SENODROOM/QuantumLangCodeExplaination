# Quantum Language Compiler - Main Entry Point

## Overview

`src/main.cpp` serves as the central entry point for the Quantum Language compiler, orchestrating the entire compilation process. This file interfaces with various components such as the lexer, parser, compiler, virtual machine (VM), disassembler, type checker, and error handler to transform source code into either executable programs or bytecode files.

### Key Design Decisions and Why

The main entry point of the compiler is designed to handle different modes based on build definitions:
- **QUANTUM_MODE_COMPILER**: Compiles `.sa` files into `.exe` files and then runs them.
- **QRUN_MODE**: Always interprets the code without bundling it into an executable.
- **Neither Mode**: Produces a standalone bundled executable, typically used for simple scripts like `hello.exe`.

This design allows flexibility in how the compiler operates, catering to both development and production environments.

### Major Classes/Functions Overview

#### Classes

1. **Lexer**: Responsible for breaking down the source code into tokens.
2. **Parser**: Converts tokens into an abstract syntax tree (AST).
3. **Compiler**: Translates the AST into bytecode.
4. **VM**: Executes the bytecode.
5. **Disassembler**: Converts bytecode back into human-readable assembly code.
6. **TypeChecker**: Ensures that the types used in the program are consistent and valid.
7. **Error**: Handles errors during the compilation process.
8. **Value**: Represents data values in the language.
9. **Serializer**: Serializes and deserializes bytecode.

#### Functions

- **getExecutablePath()**: Retrieves the path of the currently executing executable.
- **loadEmbeddedBytecode(const std::string &exePath)**: Loads embedded bytecode from the executable file. The bytecode is appended after the Portable Executable (PE) image and includes a payload size and a magic string to identify its presence.
- **printBanner()**: Prints a colorful banner to the console, providing visual feedback about the compiler's operation.

### Tradeoffs

- **Flexibility vs. Simplicity**: The ability to switch between different modes (compiler, interpreter, bundled executable) adds complexity but provides more options for users.
- **Performance vs. Development Time**: Bundling bytecode into an executable can improve performance but may increase development time due to additional steps involved in creating the executable.
- **Memory Usage vs. Execution Speed**: Storing bytecode in memory allows for faster execution compared to interpreting source code directly, but requires more memory.

Overall, `src/main.cpp` plays a crucial role in managing the flow of the compilation process and ensuring that the correct mode is selected based on the build configuration. Its modular design makes it easier to extend and maintain the compiler in the future.