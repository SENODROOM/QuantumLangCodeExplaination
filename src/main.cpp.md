# Quantum Language Compiler - Main Entry Point

## Overview

`src/main.cpp` is the primary entry point for the Quantum Language compiler. This file orchestrates the entire compilation process, managing the initialization and interaction between various components including the lexer, parser, interpreter, type checker, and error handler. It also handles command-line arguments and directs the execution flow based on user inputs.

## Key Design Decisions

### Modular Architecture

**WHY:** By adopting a modular architecture, the Quantum Language compiler can be easily extended and maintained. Each component (lexer, parser, interpreter, etc.) operates independently, allowing developers to focus on specific aspects without impacting others. This separation enhances testability and scalability, making it easier to add new features or fix bugs in isolated parts of the compiler.

### Command-Line Argument Handling

**WHY:** Implementing robust command-line argument handling ensures flexibility and usability. Users can specify options such as input files, output formats, and testing modes directly from the command line, providing a powerful interface for controlling the compiler's behavior. This approach avoids hardcoding configuration settings within the source code, promoting better maintainability and adaptability.

## Major Classes and Functions

### `printBanner()`

**Purpose:** Prints the banner text at the start of the program, introducing the Quantum Language compiler.

**Behavior:** Outputs a colorful ASCII art banner with the version information and a brief description of the language.

### `printAura()`

**Purpose:** Displays an achievement summary, highlighting the key milestones and features implemented in the Quantum Language compiler.

**Behavior:** Outputs a colorful ASCII art section listing completed features like C++17 compiler implementation, tree-walk interpreter engine, lexical analysis, recursive descent parser, abstract syntax tree, dynamic type system, and REPL interactive mode.

### Global Variables

- **`g_testMode`**: A boolean flag used during testing to make the `input()` function return immediately instead of blocking on standard input.

## Tradeoffs and Limitations

- **Performance:** While the modular design promotes scalability, it may introduce some overhead due to increased function calls and data transfers between modules.
- **Complexity:** With multiple components interacting, the overall complexity of the project increases, which can lead to more challenging debugging and maintenance tasks.
- **Resource Usage:** Running the Quantum Language compiler might consume more memory and processing power compared to simpler compilers, especially when dealing with large programs or complex ASTs.

## Conclusion

`src/main.cpp` plays a crucial role in coordinating the various components of the Quantum Language compiler, ensuring a smooth and efficient compilation process. Its modular architecture and flexible command-line argument handling contribute significantly to its scalability and usability. However, these advantages come with tradeoffs in terms of performance, complexity, and resource usage.