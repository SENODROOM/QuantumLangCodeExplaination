# Quantum Language Compiler - Main Entry Point

## Overview

`src/main.cpp` serves as the primary entry point for the Quantum Language compiler, overseeing the complete compilation process. This includes interfacing with components such as the lexer, parser, compiler, virtual machine (VM), disassembler, type checker, and error handler to convert source code into an executable program or bytecode.

## Role in Compiler Pipeline

- **Lexer**: Converts raw text into tokens.
- **Parser**: Constructs an abstract syntax tree (AST) from tokens.
- **Compiler**: Translates AST into intermediate representation (IR).
- **Virtual Machine (VM)**: Executes IR.
- **Disassembler**: Converts bytecode back into human-readable format.
- **Type Checker**: Ensures types are consistent throughout the code.
- **Error Handler**: Manages and reports errors during compilation.

## Key Design Decisions and Why

- **Modular Architecture**: Each component (lexer, parser, compiler, etc.) operates independently, facilitating easier maintenance and scalability.
- **Platform Independence**: Designed to work across multiple platforms, ensuring broad compatibility.
- **Test Mode Support**: Allows for running the compiler in test mode, which can be useful for debugging and development purposes.

## Major Classes/Functions Overview

### `getExecutablePath()`
- **Purpose**: Retrieves the path of the currently executing executable.
- **Why**: Necessary for loading embedded bytecode from the same executable.

### `loadEmbeddedBytecode(const std::string &exePath)`
- **Purpose**: Loads embedded bytecode from the specified executable path.
- **How**: Reads the file, checks for a specific magic number, extracts the payload size, and then deserializes the payload.
- **Why**: Enables the creation of standalone executables that contain both the runtime and the compiled bytecode.

### `printBanner()`
- **Purpose**: Prints a banner to the console when the compiler starts.
- **Why**: Provides visual feedback and enhances user experience.

## Tradeoffs

- **Complexity vs. Simplicity**: The modular architecture adds complexity but improves maintainability and extensibility.
- **Performance vs. Resource Usage**: Optimizing performance often requires more resources, and vice versa.
- **Flexibility vs. Consistency**: Allowing for different modes (compiler, interpreter, stub) provides flexibility but may introduce inconsistencies.

This README.md provides a comprehensive overview of the `src/main.cpp` file, detailing its role in the compiler pipeline, key design decisions, major functions/classes, and potential tradeoffs.