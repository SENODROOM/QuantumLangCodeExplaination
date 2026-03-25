# Quantum Language Compiler - Main Entry Point

## Overview

`src/main.cpp` serves as the central component of the Quantum Language compiler, orchestrating the entire compilation process. This includes managing interactions with the lexer, parser, compiler, virtual machine (VM), disassembler, type checker, and error handler. It also handles command-line arguments and directs the execution flow based on the build mode specified.

### Key Design Decisions and Trade-offs

The design of `main.cpp` involves several critical decisions:

- **Modular Architecture**: The compiler is divided into distinct modules (`Lexer`, `Parser`, `Compiler`, `VM`, `Disassembler`, `TypeChecker`, `Error`). Each module has its specific responsibilities, ensuring a clean separation of concerns and easier maintenance.

- **Build Modes**: Depending on the build mode defined (`QUANTUM_MODE_COMPILER`, `QRUN_MODE`, or none), the executable behaves differently:
  - **QUANTUM_MODE_COMPILER**: Compiles `.sa` files to `.exe` and then runs them.
  - **QRUN_MODE**: Always interprets the code, never bundles it.
  - **Neither Mode**: Produces a standalone bundled executable like `hello.exe`.

  This decision allows flexibility in how the compiler operates without requiring significant changes to the core logic.

- **Windows Bundling**: For Windows builds, the executable can include embedded bytecode. This feature simplifies deployment but adds complexity during runtime due to the need to extract and execute the embedded code.

- **Command-Line Arguments**: `main.cpp` processes command-line arguments to determine the input file, output file, and other options. This ensures that users can customize the behavior of the compiler easily.

### Major Classes/Functions Overview

#### Classes

- **Lexer**: Responsible for breaking down source code into tokens.
- **Parser**: Converts tokens into an abstract syntax tree (AST).
- **Compiler**: Translates the AST into bytecode.
- **VM**: Executes the bytecode.
- **Disassembler**: Converts bytecode back into human-readable form.
- **TypeChecker**: Ensures type safety during compilation.
- **Error**: Handles errors and provides user-friendly messages.

#### Functions

- **getExecutablePath()**: Retrieves the path of the currently executing executable.
- **loadEmbeddedBytecode(const std::string &exePath)**: Loads and deserializes embedded bytecode from the executable.
- **printBanner()**: Displays the banner text for the Quantum Language compiler.

### Role in Compiler Pipeline

The primary role of `src/main.cpp` is to coordinate the various stages of the compilation process:

1. **Lexical Analysis**: Uses the `Lexer` class to tokenize the input source code.
2. **Syntactic Analysis**: Utilizes the `Parser` class to parse the tokens into an AST.
3. **Semantic Analysis**: The `TypeChecker` class performs semantic checks on the AST to ensure type safety.
4. **Code Generation**: The `Compiler` class translates the AST into bytecode.
5. **Execution**:
   - If in `QUANTUM_MODE_COMPILER`, the bytecode is serialized and written to an output file, which is then executed by the VM.
   - If in `QRUN_MODE`, the bytecode is directly interpreted by the VM.
   - In any other mode, the VM executes the standalone bundled bytecode.

### Usage Example

To compile and run a Quantum Language script named `example.sa`, you would typically use the following command:

```sh
./quantum example.sa
```

This command triggers the lexical analysis, syntactic analysis, semantic analysis, code generation, and execution phases managed by `src/main.cpp`.

For always interpreting a script:

```sh
./qrun example.sa
```

And for generating a standalone executable:

```sh
./quantum_stub example.sa
```

These commands allow users to control the behavior of the compiler according to their needs.

By understanding these components and their roles, developers can effectively extend and modify the Quantum Language compiler to meet new requirements or improve existing functionalities.