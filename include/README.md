# QuantumLanguage Compiler - Pipeline.h

## Overview

The `include/Pipeline.h` header file is an essential component of the QuantumLanguage compiler, focusing on the compilation process pipeline. This file outlines the steps involved in compiling source code into executable bytecode, ensuring efficient execution and robust error handling.

## Compilation Pipeline

1. **Lexer**: Converts the source code into tokens.
2. **Parser**: Parses these tokens into a structured abstract syntax tree (AST).
3. **Type-Checker**: Validates the AST for type correctness, issuing warnings but not errors.
4. **Bytecode Compiler**: Translates the validated AST into bytecode instructions.

The pipeline is designed to be modular and extensible, allowing for easy integration of new features or optimizations.

## Key Design Decisions

- **Modularity**: Each stage of the compilation process is encapsulated within its own class or function, promoting code reuse and maintainability.
- **Error Handling**: Warnings are issued during the type-checking phase without halting the compilation process, allowing for more flexible error management.
- **Debug Mode**: A global flag `g_testMode` enables testing by providing canned input values instead of blocking on standard input, facilitating automated testing environments.

## Major Classes/Functions Overview

### `compileSource`

- **Purpose**: Compiles a given source code string into bytecode.
- **Parameters**:
  - `const std::string &source`: The source code to compile.
  - `const std::string &sourcePath = "<input>"`: Path to the source file (default is "<input>").
  - `bool debug = false`: Enables debug mode if set to true.
- **Returns**: A `std::shared_ptr<Chunk>` representing the compiled bytecode chunk.

### `g_testMode`

- **Role**: Global flag used during batch testing to simulate user input.
- **Usage**: Controlled by the test runner, it influences the behavior of functions like `input()` in `VmNatives.cpp`.

## Tradeoffs

- **Flexibility vs. Complexity**: While modularity enhances flexibility, it also increases complexity due to the need for coordination between different stages of the pipeline.
- **Performance vs. Error Handling**: Allowing warnings during type checking improves performance but may compromise the strictness of error handling.
- **Automation vs. User Interaction**: Enabling `g_testMode` facilitates automation but removes the possibility of interactive user input.

This file is critical for the overall functionality and efficiency of the QuantumLanguage compiler, ensuring that the compilation process is both effective and adaptable to various needs and environments.