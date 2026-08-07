# Quantum Language Compiler - Main Entry Point

## Overview

`src/main.cpp` serves as the central entry point for the Quantum Language compiler, orchestrating the entire compilation process. It interfaces with various components such as the lexer, parser, compiler, virtual machine (VM), disassembler, type checker, and error handler to transform source code into either an executable program or a standalone runtime environment.

## Role in Compiler Pipeline

The `main.cpp` file acts as the glue that connects different stages of the compiler pipeline:

1. **Lexer**: Converts raw text into tokens.
2. **Parser**: Parses tokens into a structured abstract syntax tree (AST).
3. **Type Checker**: Validates the types within the AST.
4. **Compiler**: Translates the validated AST into intermediate representation (IR) or bytecode.
5. **Virtual Machine (VM)**: Executes the IR or bytecode.
6. **Disassembler**: Converts bytecode back into human-readable assembly code.
7. **Error Handler**: Manages and reports errors encountered during the compilation process.

## Key Design Decisions and Trade-offs

### Build Modes

- **QUANTUM_MODE_COMPILER**: Compiles `.sa` files into `.exe` files and runs them immediately.
- **QRUN_MODE**: Always interprets the code without bundling it into an executable.
- **Neither Mode Defined**: Runs as a standalone bundled executable (`quantum_stub.exe`).

**Trade-off**: Different modes cater to various use cases—compilation and immediate execution, pure interpretation, and standalone deployment. Each mode has specific advantages and limitations:
- **QUANTUM_MODE_COMPILER**: Provides convenience for users who want to compile and run their programs directly.
- **QRUN_MODE**: Ensures that the program is always interpreted, which can be useful for debugging and testing.
- **Neither Mode Defined**: Offers a lightweight, self-contained runtime suitable for applications like `hello.exe`.

### Platform-Specific Code

The file includes platform-specific code to handle console output encoding on Windows:

```cpp
#ifdef _WIN32
    SetConsoleOutputCP(CP_UTF8);
    SetConsoleCP(CP_UTF8);
#endif
```

**Why**: Setting the console output and input codes to UTF-8 ensures proper display of Unicode characters on Windows systems.

### Error Handling

The file uses custom exception handling to manage errors effectively:

```cpp
catch (const QuantumError &e)
{
    std::cerr << Colors::RED << "[" << e.kind << "] " << Colors::RESET << e.what() << "\n";
    return 1;
}
catch (const std::exception &e)
{
    std::cerr << Colors::RED << "[Fatal] " << Colors::RESET << e.what() << "\n";
    return 1;
}
```

**Why**: Custom exceptions allow for more granular control over error reporting, making it easier to identify and address issues in the compiled code.

## Major Classes/Functions Overview

### Classes

- **CLI**: Handles command-line interface operations, including running the REPL and displaying help messages.
- **Pipeline**: Orchestrates the flow of data through the compiler pipeline, managing the conversion from source code to executable.
- **Dialect**: Manages language dialects and provides functionality for parsing and compiling different languages.
- **Disassembler**: Converts bytecode back into assembly code for debugging purposes.
- **VM**: Executes the compiled bytecode.
- **Error**: Manages error reporting and handling.

### Functions

- **getExecutablePath()**: Retrieves the path of the currently executing executable.
- **loadEmbeddedBytecode(std::string exePath)**: Loads embedded bytecode from the executable.
- **runREPL()**: Starts the Read-Eval-Print Loop (REPL) for interactive programming.
- **printBanner()**: Displays the banner for the Quantum Language compiler.
- **printHelp(std::string appName)**: Prints help information for the specified application.

## Conclusion

`src/main.cpp` is a critical component of the Quantum Language compiler, providing the necessary infrastructure to handle different build modes, parse and compile source code, execute bytecode, and manage errors. Its design allows for flexibility and adaptability across various use cases, ensuring that the compiler can meet the needs of both developers and end-users efficiently.