# QuantumLanguage Compiler - Cli.h

## Overview

The `include/Cli.h` header file is integral to the QuantumLanguage compiler, focusing on the command-line interface (CLI) functionalities. This includes running individual files, interacting with the Read-Eval-Print Loop (REPL), executing batch tests, and bundling standalone executables. The CLI ensures seamless interaction between the user and the compiler, providing both interactive and batch processing capabilities.

## Role in Compiler Pipeline

In the QuantumLanguage compiler's pipeline, `Cli.h` serves as the entry point for all command-line operations. It handles the parsing of user inputs, invokes the appropriate compiler components, and manages the execution flow based on the specified commands. For instance, when a user compiles a file using the command-line interface, `Cli.h` initiates the compilation process, loads the necessary resources, and outputs the results.

## Key Design Decisions and WHY

1. **Separation of Concerns**: The CLI functions are designed to be modular and independent, allowing each component to focus on its specific task. This separation enhances maintainability and scalability of the compiler.

2. **Error Handling**: Each function in `Cli.h` includes robust error handling mechanisms. For example, `runFile` exits non-zero on error, ensuring that any issues during file interpretation are immediately apparent to the user. Similarly, `runTestExamples` provides a structured marker line for errors, making debugging easier.

3. **Debugging Support**: Both `runFile` and `runREPL` support debugging mode, which allows users to step through the code, inspect variables, and understand the program's execution flow. This feature is particularly useful for developers during the testing and development phases.

4. **Batch Processing**: The `runTestExamples` function facilitates batch testing by running every supported source file under a specified directory. This is essential for verifying the correctness of multiple files simultaneously.

5. **Standalone Executable Bundling**: The `bundleAndRun` function combines the compiler's functionality with the ability to create standalone executables. This feature simplifies deployment and distribution, making it easier for users to run their programs without additional setup.

## Major Classes/Functions Overview

### Console Output Functions
- **printBanner()**: Displays the compiler banner.
- **printAura()**: Prints a decorative pattern.
- **printHelp(const char *prog)**: Provides help information for the specified program.

### File Execution Functions
- **runFile(const std::string &path, bool debug = false)**: Interprets a single file in-place. If `debug` is true, it enables debugging mode.
- **checkFile(const std::string &path)**: Parses and type-checks a file, returning a process exit code.

### REPL Function
- **runREPL(bool debug = false)**: Starts the Read-Eval-Print Loop, enabling interactive programming with optional debugging mode.

### Batch Test Runner Functions
- **runTestExamples(const std::string &dir)**: Runs all supported source files under the specified directory, writing results to `test_results.txt`.
- **runSingleFileForTest(const std::string &path)**: Hidden function used by the batch test runner to execute a single file safely within a new process.

### Standalone .exe Bundling Functions
- **getExecutablePath()**: Retrieves the path of the currently running executable.
- **loadEmbeddedBytecode(const std::string &exePath)**: Loads bytecode embedded in an executable, returning a shared pointer to the chunk or nullptr if absent.
- **bundleAndRun(const std::string &path, const std::string &exePath)**: Compiles a file, copies `quantum_stub.exe`, appends the bytecode payload, and runs the resulting executable.

## Tradeoffs

1. **Complexity vs. Usability**: While the CLI offers extensive features, including debugging and batch testing, it also increases complexity. Users must learn how to utilize these features effectively, which might be challenging for beginners.

2. **Performance vs. Debugging**: Enabling debugging mode can significantly slow down the execution speed due to additional checks and outputs. However, this tradeoff is often acceptable for development and testing purposes.

3. **Resource Usage vs. Efficiency**: Bundling standalone executables requires additional resource usage, such as copying external binaries and appending bytecode. While this improves ease of use, it may impact performance and storage efficiency.

Overall, `Cli.h` plays a vital role in enhancing the usability and flexibility of the QuantumLanguage compiler, despite some potential tradeoffs in terms of complexity and performance.