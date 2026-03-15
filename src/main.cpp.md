# Quantum Language Compiler - Main Entry Point

## Overview

`src/main.cpp` is the main entry point for the Quantum Language compiler. It orchestrates the entire compilation process by initializing various components such as the lexer, parser, interpreter, type checker, and error handler. This file also handles command-line arguments and manages the execution flow based on user inputs.

## Key Design Decisions

### Modular Architecture

The decision to use a modular architecture was driven by the need to separate concerns and improve maintainability. Each component (lexer, parser, interpreter, etc.) has its own header and implementation files, making it easier to manage and scale individual features without affecting others.

**Why:** This approach enhances modularity, allowing developers to work independently on different parts of the compiler.

### Error Handling

A robust error handling system was implemented using exceptions and custom error classes (`Error`). This allows for clear and consistent error reporting throughout the compiler.

**Why:** Proper error handling is crucial for debugging and providing meaningful feedback to users.

### Command-Line Arguments

The program accepts command-line arguments to control its behavior, such as enabling test mode (`--test`) and specifying input files.

**Why:** Allowing customization through command-line arguments makes the compiler more flexible and suitable for both development and production environments.

## Classes and Functions Documentation

### `printBanner()`

- **Purpose:** Prints the banner at the start of the program, introducing the Quantum Language compiler.
- **Behavior:** Outputs a colorful ASCII art banner followed by version information.

### `printAura()`

- **Purpose:** Displays achievements related to the Quantum Language compiler.
- **Behavior:** Outputs a list of completed milestones in a visually appealing format.

### Global Variables

- **`g_testMode`**: A boolean flag indicating whether the program is running in test mode. When set to `true`, the `input()` function will return an empty string immediately, bypassing any blocking behavior.

## Tradeoffs and Limitations

- **Modular Design Over Simplicity:** While the modular approach improves maintainability, it may introduce additional complexity and overhead compared to a monolithic design.
- **Error Handling Over Performance:** The emphasis on comprehensive error handling can lead to performance penalties, especially in critical sections of the compiler.
- **Command-Line Arguments Over Hardcoded Inputs:** Although flexible, the reliance on command-line arguments might make the compiler less convenient for quick testing or integration into other systems.

## Usage

To compile and run the Quantum Language compiler, execute the following commands:

```sh
mkdir build
cd build
cmake ..
make
./quantum_language [options] [file]
```

### Options

- `--test`: Enables test mode, causing the `input()` function to return an empty string immediately.
- `[file]`: Specifies the input file to be compiled. If not provided, the compiler will enter REPL mode.

### Example

```sh
./quantum_language --test example.qc
```

This command will run the compiler in test mode, processing the `example.qc` file.

## Conclusion

`src/main.cpp` serves as the central hub for the Quantum Language compiler, coordinating the interactions between various components. Its modular design, comprehensive error handling, and customizable command-line options make it a versatile tool for both development and production use. However, these features come with their respective tradeoffs, which should be considered when evaluating the overall performance and usability of the compiler.