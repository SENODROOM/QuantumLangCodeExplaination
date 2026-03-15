# Quantum Language Compiler - Main Entry Point

## Overview

`src/main.cpp` serves as the central hub for the Quantum Language compiler, managing the overall compilation process. It initializes essential components such as the lexer, parser, interpreter, type checker, and error handler. Additionally, it processes command-line arguments and controls the execution flow based on user inputs.

## Key Design Decisions

### Modular Architecture

**WHY:** Adopting a modular architecture enhances maintainability and scalability. By breaking down the compiler into distinct modules, each component can be developed, tested, and modified independently, reducing complexity and potential bugs.

### Command-Line Argument Handling

**WHY:** Implementing robust command-line argument handling allows users to control various aspects of the compilation process, such as enabling test mode, specifying input files, and setting output options. This flexibility is crucial for both development and production use cases.

### Error Handling

**WHY:** Comprehensive error handling ensures that the compiler can gracefully report issues encountered during parsing, type checking, and interpretation. This helps in debugging and improving the reliability of the compiler.

## Documentation of Major Classes/Functions

### `printBanner()`

**Purpose:** Prints the banner text at the start of the program, providing visual identification and version information.

**Behaviour:** Displays a colorful ASCII art banner along with the version and description of the Quantum Language compiler.

### `printAura()`

**Purpose:** Prints the aura text, highlighting achievements and features of the Quantum Language compiler.

**Behaviour:** Outputs a colorful ASCII art section listing significant milestones and capabilities of the compiler.

### `main(int argc, char* argv[])`

**Purpose:** The main entry point of the Quantum Language compiler. It orchestrates the entire compilation process by initializing necessary components and processing user inputs.

**Behaviour:**
1. **Initialization:** Initializes the lexer, parser, interpreter, type checker, and error handler.
2. **Command-Line Processing:** Parses command-line arguments to determine the mode of operation (e.g., interactive, batch).
3. **Execution Flow Control:** Based on the user's input, either starts the interactive mode (REPL) or processes one or more input files through the batch mode.

## Tradeoffs or Limitations

### Test Mode

While test mode provides an instant way to return empty input, it may not accurately simulate real-world user interactions, potentially leading to missed edge cases in testing.

### File Path Validation

The current implementation does not validate file paths thoroughly, which could lead to runtime errors if invalid paths are provided.

### Memory Management

The compiler uses dynamic memory allocation extensively, which can lead to memory leaks if not properly managed. Careful attention must be paid to ensure all dynamically allocated resources are freed appropriately.

## Usage Example

To compile a single file using the Quantum Language compiler:

```sh
./quantum_compiler path/to/input.qs
```

To run the interactive mode (REPL):

```sh
./quantum_compiler --interactive
```

## Contributing

Contributions to the Quantum Language compiler are welcome. Please follow these guidelines:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature/YourFeatureName`).
3. Make your changes and commit them (`git commit -m 'Add some feature'`).
4. Push to the branch (`git push origin feature/YourFeatureName`).
5. Open a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.