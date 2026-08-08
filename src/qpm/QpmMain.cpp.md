# Quantum Package Manager (QPM)

## Overview

**qpm** is a standalone, from-scratch package manager designed to be compatible with npm but without requiring npm or Node.js to be installed on the system. It directly communicates with the npm registry via HTTPS using WinHTTP and handles the unpacking of tarballs using zlib and a custom tar reader. This makes **qpm** an efficient tool for managing JavaScript project dependencies on Windows platforms.

## Role in Compiler Pipeline

While **qpm** does not fit into the traditional sense of a compiler pipeline, it can be considered a pre-build step for JavaScript projects. It ensures that all necessary dependencies are downloaded and installed before the actual compilation or execution process begins.

## Key Design Decisions and Why

### Direct Communication with npm Registry

**qpm** communicates directly with the npm registry over HTTPS using WinHTTP. This decision was made to avoid the overhead of invoking external tools like npm, which might not be available on all systems. By handling the communication internally, **qpm** provides a seamless experience for users who want to manage their JavaScript projects without additional installations.

### Custom Tar Reader

To handle the unpacking of tarballs, **qpm** uses a custom tar reader instead of relying on existing libraries. This approach allows **qpm** to have full control over the unpacking process, ensuring compatibility and efficiency across different platforms.

### No Built-in JS Engine

Unlike some other package managers, **qpm** does not include a built-in JavaScript engine. Instead, it relies on the user's environment to run scripts. This decision was made to keep **qpm** lightweight and avoid potential conflicts with existing JavaScript engines.

## Major Classes/Functions Overview

### QpmResolver

- **Role**: Responsible for resolving dependency trees based on the `package.json` file.
- **Why**: Ensures that all required packages are correctly identified and installed.
- **Overview**: The class parses the `package.json` file, fetches metadata from the npm registry, and constructs a dependency graph.

### QpmScripts

- **Role**: Handles the execution of scripts defined in the `package.json` file.
- **Why**: Allows users to run specific scripts associated with their project, such as `start`, `test`, etc.
- **Overview**: The class reads the `package.json` file, identifies the script to run, and executes it using the system shell.

### Main Function

- **Role**: Parses command-line arguments and delegates tasks to the appropriate functions.
- **Why**: Provides a simple interface for users to interact with **qpm**, allowing them to perform actions like installing dependencies or running scripts.
- **Overview**: The main function checks the command-line arguments and calls either `printHelp`, `runInstall`, `runScript`, or `runScript` with the specified script name.

## Tradeoffs

### Ease of Use vs. Dependency Management

By avoiding the need for npm or Node.js, **qpm** simplifies the installation process for users. However, this also means that it lacks some advanced features provided by these tools, such as automatic updates and version management.

### Performance vs. Complexity

Using a custom tar reader allows **qpm** to optimize the unpacking process for its specific use case. However, this complexity comes at the cost of maintainability and potential bugs.

### Security vs. Convenience

Direct communication with the npm registry eliminates the need for an intermediary tool, potentially improving security. However, this also means that **qpm** must ensure that it properly validates and sanitizes input to prevent vulnerabilities.

In summary, **qpm** is a powerful and efficient tool for managing JavaScript project dependencies on Windows platforms. While it may lack some features found in more established package managers, it offers a simpler and more secure alternative for those who prefer to avoid additional installations.