# Quantum Language Compiler - Main Entry Point

## Overview

`src/main.cpp` is the central entry point for the Quantum Language compiler, responsible for orchestrating the entire compilation process. It interfaces with various components including the lexer, parser, compiler, virtual machine (VM), disassembler, type checker, and error handler to transform source code into either executable programs or standalone bundled executables.

## Key Design Decisions and Why

The main design decision in `src/main.cpp` is its modularity and separation of concerns. Each component handles specific aspects of the compilation process, ensuring that the system remains organized and scalable. The choice to support different modes (`QUANTUM_MODE_COMPILER`, `QRUN_MODE`) allows flexibility in how the compiled output can be used, whether for direct execution or interpretation.

### Tradeoffs

- **Modularity vs. Complexity**: By breaking down the compilation process into multiple modules, the code becomes more complex but easier to maintain and extend.
- **Performance vs. Memory Usage**: Some optimizations might sacrifice memory usage for performance, which is particularly relevant when dealing with large source files or complex programs.
- **Flexibility vs. Simplicity**: Supporting both compilation and direct execution requires additional complexity, but provides greater flexibility in how the language can be used.

## Major Classes/Functions Overview

### `getExecutablePath()`
- **Role**: Retrieves the full path of the currently executing program.
- **Why**: Necessary for locating embedded bytecode within the executable itself.
- **Tradeoffs**: No significant tradeoffs; straightforward utility function.

### `loadEmbeddedBytecode(const std::string &exePath)`
- **Role**: Loads and deserializes embedded bytecode from an executable file.
- **Why**: Allows the compiler to include precompiled code directly within the binary, reducing startup time and disk space requirements.
- **Tradeoffs**: Requires careful handling of file formats and potential security implications.

### `printBanner()`
- **Role**: Prints a banner or logo to the console.
- **Why**: Enhances user experience by providing visual identification of the tool.
- **Tradeoffs**: Minimal impact on functionality; purely aesthetic.

## Role in Compiler Pipeline

`src/main.cpp` acts as the glue between all other components. It initializes the necessary objects, reads input files, invokes the appropriate methods for parsing, compiling, and running, and finally cleans up resources. This ensures that each step of the compilation process is executed in the correct order and with the required dependencies.

## Conclusion

`src/main.cpp` is a critical file in the Quantum Language compiler, driving the overall compilation workflow. Its modular design and focus on specific tasks make it a robust and flexible solution for transforming source code into executable programs. While it introduces some complexity, the benefits in terms of maintainability and functionality outweigh these tradeoffs.