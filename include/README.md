# QuantumLanguage Compiler - Interpreter.h

## Overview

The `include/Interpreter.h` header file is an essential part of the QuantumLanguage compiler, focusing on the execution phase of the language. This file defines the `Interpreter` class, which is responsible for interpreting and executing abstract syntax tree (AST) nodes according to the rules of the QuantumLanguage. The `Interpreter` class encapsulates the logic needed to traverse the AST and perform operations based on the node types, including variable declarations, function calls, control structures, and more. By leveraging C++'s type system and features like `std::variant`, it ensures flexibility and efficiency in handling different value types during interpretation.

## Key Design Decisions

### Use of `std::variant` for Value Types

**WHY:** Using `std::variant` allows the `Interpreter` to manage multiple value types within a single container without resorting to unions or pointers. This approach enhances safety and readability while providing a clear and expressive way to represent the diverse data types encountered in QuantumLanguage.

### Environment Management with `std::shared_ptr`

**WHY:** Managing environments using smart pointers (`std::shared_ptr`) helps in maintaining reference counts and automatically deallocating memory when environments go out of scope. This simplifies memory management and prevents resource leaks, making the interpreter robust and scalable.

### Static Analysis for Loop Guards

**WHY:** Implementing a static analysis mechanism to guard against infinite loops through a fixed maximum step count (`MAX_STEPS`) provides a simple yet effective safeguard. While not foolproof, it significantly reduces the risk of running into infinite loops due to erroneous or malicious code, ensuring the interpreter remains stable under various conditions.

## Classes and Functions Documentation

### Class: `Interpreter`

**Purpose:** The primary class responsible for interpreting and executing AST nodes.

**Behavior:**
- **Constructor:** Initializes the interpreter with global and local environments.
- **Methods:**
  - `execute(ASTNode &node):` Executes the given AST node.
  - `evaluate(ASTNode &node):` Evaluates the given AST node and returns its value.
  - `execBlock(BlockStmt &s, std::shared_ptr<Environment> scope = nullptr):` Executes a block statement within a specified environment.
  - `registerNatives():` Registers native functions available to the interpreter.
  - Various statement executor methods (`execVarDecl`, `execFunctionDecl`, etc.) handle specific types of AST statements.
  - Various expression evaluator methods (`evalBinary`, `evalUnary`, etc.) handle evaluation of different AST expressions.
  - Methods for evaluating C++ pointer expressions (`evalAddressOf`, `evalDeref`, etc.).
  - Method dispatchers for calling built-in methods on objects (`callMethod`, `callArrayMethod`, etc.).

### Function: `callFunction`

**Purpose:** Calls a user-defined function with provided arguments.

**Behavior:** Takes a shared pointer to a `QuantumFunction` and a vector of `QuantumValue` arguments, then executes the function, passing the arguments and returning the result.

### Function: `callNative`

**Purpose:** Calls a native function with provided arguments.

**Behavior:** Similar to `callFunction`, but specifically handles native functions registered with the interpreter.

### Function: `callInstanceMethod`

**Purpose:** Calls a method on an instance object with provided arguments.

**Behavior:** Takes a shared pointer to an instance object, a shared pointer to a `QuantumFunction`, and a vector of `QuantumValue` arguments, then invokes the method on the instance.

### Function: `setLValue`

**Purpose:** Sets the value of an l-value target node.

**Behavior:** Handles setting values for variables, array elements, dictionary keys, and other l-values, supporting both assignment and compound assignment operators.

## Tradeoffs and Limitations

- **Static Step Guard:** The use of a fixed maximum step count for loop detection is a simple solution but may not cover all potential cases of infinite loops.
- **Memory Management:** Smart pointers provide automatic memory management, but they introduce overhead compared to raw pointers. Careful consideration must be given to balancing safety and performance.
- **Type Safety:** `std::variant` offers strong type safety, but it requires compile-time knowledge of all possible types, which might limit the flexibility of adding new types in the future.

This README.md provides a comprehensive overview of the `Interpreter.h` file, detailing its role, key design decisions, and the functionality of its classes and functions. It also acknowledges the tradeoffs and limitations inherent in the implementation, offering insights into areas where further optimization or flexibility could be beneficial.