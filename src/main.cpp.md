# Quantum Language Compiler - Main Entry Point

## Overview

`src/main.cpp` serves as the central hub for the Quantum Language compiler, managing the overall compilation process. It initializes essential components like the lexer, parser, interpreter, type checker, and error handler. Additionally, it processes command-line arguments and controls the execution flow based on user inputs.

## Key Design Decisions

### Modular Architecture

**WHY:** Adopting a modular architecture enhances maintainability and scalability. Each component (lexer, parser, etc.) can be developed, tested, and modified independently without affecting others. This separation allows for easier debugging and updates, ensuring that changes in one area do not disrupt the functionality of other parts of the compiler.

### Command-Line Argument Handling

**WHY:** Implementing robust command-line argument handling provides flexibility and control over the compiler's behavior. Users can specify options such as input files, output formats, and test modes directly from the command line, making the compiler more versatile and user-friendly.

### Error Handling and Reporting

**WHY:** Comprehensive error handling ensures that issues are identified and reported accurately, aiding developers in diagnosing problems efficiently. By providing detailed error messages and context, the compiler helps users understand where errors occurred and how they can be resolved.

## Major Classes and Functions

### `printBanner()`
- **PURPOSE:** Displays the banner text at the start of the program, introducing the Quantum Language compiler.
- **BEHAVIOUR:** Outputs a colorful ASCII art banner along with version information.

### `printAura()`
- **PURPOSE:** Highlights achievements and features of the Quantum Language compiler.
- **BEHAVIOUR:** Prints a list of completed milestones and features using colored text.

### `main(int argc, char* argv[])`
- **PURPOSE:** The primary function that drives the Quantum Language compiler.
- **BEHAVIOUR:**
  - Parses command-line arguments to determine the mode of operation (e.g., compiling a file, running in interactive mode).
  - Initializes the lexer, parser, interpreter, type checker, and error handler.
  - Reads the input file and processes it according to the selected mode.
  - Handles any errors encountered during the compilation process and reports them to the user.

## Tradeoffs and Limitations

- **Performance:** The current implementation may not be optimized for large-scale projects due to simplicity and ease of development.
- **Complexity:** While the modular approach improves maintainability, it adds complexity to the setup and configuration process.
- **Error Reporting:** Although detailed error messages are provided, they might not cover all edge cases or provide sufficient context for complex errors.

## Usage

To compile a Quantum Language file, run:

```sh
./quantum_compiler input.q
```

For an interactive shell, use:

```sh
./quantum_compiler --repl
```

In test mode, which is useful for automated testing, set the global variable `g_testMode` to `true`.

## Contributing

Contributions to the Quantum Language compiler are welcome! Please follow these guidelines:

1. Fork the repository.
2. Create a new branch (`feature/X` or `fix/Y`).
3. Make your changes and commit them.
4. Push to the branch (`git push origin feature/X`).
5. Open a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.