# QuantumLanguage Compiler - TypeChecker.h

## Overview

The `include/TypeChecker.h` header file plays a vital role in the QuantumLanguage compiler by ensuring type safety during the compilation process. This file defines the `StaticTypeError` exception class and the `TypeEnv` structure, which manage static type environments, and the `TypeChecker` class, which performs type checking on abstract syntax tree (AST) nodes.

## Key Design Decisions

### Use of Exception Handling

**Decision:** The `StaticTypeError` exception class is derived from `std::runtime_error`. This decision was made to provide a clear and structured way to handle static type errors, allowing the compiler to report errors at compile time rather than runtime.

**Why:** Static type errors are easier to diagnose and fix because they occur before the program runs. By using exceptions, we can ensure that the compiler stops processing and reports an error immediately when it encounters a type mismatch.

### Utilization of `std::variant` for Value Types

**Decision:** Although not explicitly shown in the provided code snippet, the use of `std::variant` would likely be integrated into the `TypeChecker` to manage different data types within the language.

**Why:** Using `std::variant` allows for a flexible and efficient representation of multiple value types within a single variable. This approach helps in reducing the overhead associated with dynamic type switching and improves performance.

## Classes and Functions Documentation

### StaticTypeError Class

**Purpose:** Represents a static type error encountered during the compilation process.

**Behavior:** Inherits from `std::runtime_error` and includes the line number where the error occurred.

**Trade-offs:** While providing detailed error information, this approach might lead to more verbose error messages compared to simpler error handling mechanisms.

### TypeEnv Structure

**Purpose:** Manages static type environments, storing variable names and their corresponding types.

**Behavior:** Supports defining new variables and resolving existing ones. If a variable is not found in the current environment, it searches in the parent environment.

**Limitations:** Assumes that all variable types are known at compile time. Dynamic typing scenarios are not supported.

### TypeChecker Class

**Purpose:** Performs type checking on AST nodes to ensure that the code adheres to the defined type rules.

**Behavior:** Initializes with a global type environment and provides methods to check entire lists of nodes (`check`) and individual nodes (`checkNode`). The `checkNode` method returns the type of the evaluated node based on the current environment.

**Limitations:** Does not currently support `std::variant` for value types, which could limit its flexibility in handling complex data structures.

## Tradeoffs and Limitations

- **Error Reporting:** The use of exceptions for error reporting can make the error messages more detailed but may also increase the complexity of the code.
- **Flexibility:** Assuming static typing limits the compiler's ability to handle dynamic scenarios, which might be necessary in some programming languages.
- **Performance:** Efficiently managing type environments and performing type checks can impact the overall performance of the compiler.

This file is essential for maintaining the integrity and correctness of the QuantumLanguage codebase by ensuring that all operations respect the declared types.