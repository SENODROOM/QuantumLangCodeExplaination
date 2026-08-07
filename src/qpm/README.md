# QpmScripts.h

## Role in Compiler Pipeline

`QpmScripts.h` is a header file within the Quantum Programming Module (QPM) of the Quantum Language compiler. It plays a crucial role in managing and executing scripts defined in a project's `package.json` file. This module ensures that the necessary dependencies are available in the execution environment by prepending the `node_modules/.bin` directory to the system's `PATH`.

## Key Design Decisions and WHY

### Prepending `node_modules/.bin` to PATH

**WHY**: The primary reason for this design decision is to ensure that any locally installed Node.js packages can be executed directly from the command line without requiring explicit paths. This simplifies the user experience and makes it easier to manage project-specific tools and utilities.

### Using Shell Commands

**WHY**: By spawning the command string from `package.json`'s "scripts" section as a shell command, `QpmScripts.h` leverages the existing ecosystem of shell commands and scripts. This approach allows developers to use familiar syntax and tools, ensuring compatibility and ease of use.

### Error Handling

**WHY**: Proper error handling is essential in any software component. In `QpmScripts.h`, the function returns the child process's exit code, providing a clear indication of whether the script executed successfully. If the script cannot be found, it prints an error message to `stderr` and returns an exit code of 1. This ensures that users receive immediate feedback on what went wrong.

## Major Classes/Functions Overview

### `runScript` Function

- **Purpose**: Executes a specified script from a project's `package.json` file.
- **Parameters**:
  - `const std::string &projectDir`: The directory containing the project's `package.json`.
  - `const std::string &scriptName`: The name of the script to execute.
- **Return Value**: The exit code of the child process. Returns 1 with an error message to `stderr` if the script cannot be found.
- **Functionality**:
  - Reads the `package.json` file located at `<projectDir>`.
  - Retrieves the command associated with the specified `scriptName`.
  - Prepends the `node_modules/.bin` directory to the system's `PATH`.
  - Spawns the command as a shell process.
  - Handles errors gracefully, printing an error message to `stderr` if the script is not found.

## Tradeoffs

### Simplicity vs. Flexibility

**Simplicity**: By using shell commands, `QpmScripts.h` provides a simple and straightforward interface for executing scripts. Users do not need to worry about the underlying details of how the script is executed.

**Flexibility**: However, this simplicity comes at the cost of flexibility. While shell commands offer broad support for various scripting languages and tools, they may not provide the same level of control and customization as a dedicated programming language or framework.

### Performance vs. Ease of Use

**Performance**: Executing scripts through the shell might introduce some performance overhead compared to native C++ execution. However, this tradeoff is generally acceptable given the benefits of simplicity and ease of use.

**Ease of Use**: By abstracting the execution logic away from the core compiler, `QpmScripts.h` improves the overall usability of the compiler. Developers can focus on writing quantum programs rather than worrying about script management.

In conclusion, `QpmScripts.h` is a vital component of the Quantum Language compiler, responsible for executing scripts defined in a project's `package.json` file. Its design decisions prioritize simplicity and ease of use over flexibility and performance, making it an effective solution for managing project-specific tools and utilities.