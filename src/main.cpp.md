# Quantum Language Compiler - Main Entry Point

## Overview

`src/main.cpp` serves as the primary entry point for the Quantum Language compiler. It manages the overall compilation process by coordinating the initialization of essential components such as the lexer, parser, interpreter, type checker, and error handler. Additionally, it handles command-line arguments and controls the execution flow based on user inputs.

## Key Design Decisions

### Modular Architecture

**WHY:** Implementing a modular architecture allows for better separation of concerns and easier maintenance. Each component (lexer, parser, interpreter, etc.) can be developed, tested, and modified independently without affecting the others. This also facilitates scalability and extensibility, making it easier to add new features or support additional language constructs in the future.

### Command-Line Argument Handling

**WHY:** Command-line argument handling is crucial for allowing users to interact with the compiler. By parsing these arguments, `main.cpp` can determine whether to run the compiler in normal mode, test mode, or other specific configurations. This provides flexibility and control over how the compiler operates, enabling developers to perform various tasks programmatically or through scripts.

### Error Handling and Reporting

**WHY:** Robust error handling and reporting mechanisms are vital for diagnosing issues during the compilation process. By centralizing error management in `main.cpp`, the compiler can provide clear and informative error messages to users, helping them identify and fix problems more effectively. This also ensures consistency in error reporting across different parts of the compiler.

## Classes and Functions Documentation

### printBanner()

**Purpose:** Prints the banner text at the start of the program, introducing the Quantum Language compiler.

**Behaviour:** Outputs a colorful ASCII art banner followed by version information.

### printAura()

**Purpose:** Displays an achievement summary, highlighting key milestones and features implemented in the Quantum Language compiler.

**Behaviour:** Outputs a colorful ASCII art aura with a list of completed achievements.

### main(int argc, char* argv[])

**Purpose:** The primary function of the Quantum Language compiler, responsible for managing the entire compilation process.

**Behaviour:**
1. Parses command-line arguments to determine the operating mode.
2. Initializes essential components: lexer, parser, interpreter, type checker, and error handler.
3. Reads input from files or standard input based on user configuration.
4. Coordinates the compilation steps: lexical analysis, syntax parsing, semantic analysis, and interpretation.
5. Handles errors and reports them to the user.
6. Provides an interactive REPL mode if configured.

### handleCommandlineArgs(int argc, char* argv[])

**Purpose:** Processes command-line arguments to configure the compiler's behavior.

**Behaviour:** Sets flags like `g_testMode` based on user inputs and validates provided options.

### readInput(const std::string& filename, bool isREPL)

**Purpose:** Reads the source code from either a file or standard input.

**Behaviour:** Opens a file and reads its contents into a string, or reads directly from standard input if running in REPL mode.

### compileSourceCode(const std::string& sourceCode)

**Purpose:** Compiles the source code using the initialized components.

**Behaviour:**
1. Performs lexical analysis to tokenize the source code.
2. Parses the tokens to construct an abstract syntax tree (AST).
3. Checks the AST for semantic correctness.
4. Interprets the AST to execute the quantum language code.

## Tradeoffs and Limitations

- **Modular Architecture vs. Complexity:** While a modular approach enhances maintainability, it may introduce some complexity in terms of inter-component communication and synchronization.
- **Error Handling vs. Performance:** Comprehensive error handling can slow down the compilation process, especially in cases where minor errors are encountered frequently. However, it is essential for providing accurate feedback to users.
- **REPL Mode vs. Batch Processing:** Providing an interactive REPL mode increases convenience but may limit the compiler's ability to handle large batch jobs efficiently.

This README.md provides a comprehensive overview of the `src/main.cpp` file, detailing its role in the compiler pipeline, key design decisions, and the functionality of major classes and functions. It also acknowledges potential tradeoffs and limitations associated with the implementation.