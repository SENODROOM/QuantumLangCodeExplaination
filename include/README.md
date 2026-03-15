# QuantumLanguage Compiler - Interpreter.h

## Overview

The `include/Interpreter.h` header file is an integral part of the QuantumLanguage compiler's execution phase. It defines the `Interpreter` class responsible for evaluating and executing abstract syntax tree (AST) nodes to produce runtime behavior. The file includes methods for handling different types of statements and expressions, facilitating the dynamic interpretation of the QuantumLanguage program.

## Key Design Decisions

### Use of ASTNodes for Evaluation

**Why:** By using `ASTNode` objects directly for evaluation, the interpreter can handle any type of statement or expression without needing to differentiate between them explicitly at compile time. This approach simplifies the implementation and allows for a more flexible and extensible system.

### Environment Management

**Why:** An environment (`std::shared_ptr<Environment>`) is used to manage variable bindings and scopes. This decision ensures that variables declared in different parts of the program are properly isolated and accessible, preventing name clashes and unintended side effects.

### Step Count Limitation

**Why:** To guard against infinite loops, especially in cases like reading from an empty input stream, a maximum step count (`MAX_STEPS`) is implemented. This limitation helps maintain the stability and responsiveness of the interpreter.

## Documentation of Major Classes/Functions

### Interpreter Class

**Purpose:** Manages the overall state of the interpreter, including the global environment and step count tracking.

**Behaviour:**
- **Constructor (`Interpreter()`):** Initializes the interpreter with a global environment.
- **execute (`void execute(ASTNode &node)`):** Executes a single AST node.
- **evaluate (`QuantumValue evaluate(ASTNode &node)`):** Evaluates an AST node and returns its result as a `QuantumValue`.
- **execBlock (`void execBlock(BlockStmt &s, std::shared_ptr<Environment> scope = nullptr)`):** Executes a block of statements within a specified scope.
- **registerNatives (`void registerNatives()`):** Registers built-in functions and methods available to the interpreter.
- **Statement Executors:** Methods like `execVarDecl`, `execFunctionDecl`, etc., handle specific types of statements.
- **Expression Evaluators:** Methods like `evalBinary`, `evalUnary`, etc., handle various expression types.
- **C++ Pointer Evaluators:** Methods like `evalAddressOf`, `evalDeref`, etc., handle pointer-related operations.
- **callFunction, callNative, callInstanceMethod:** Functions to invoke user-defined, native, and instance methods respectively.
- **Built-in Method Dispatch:** Methods like `callArrayMethod`, `callStringMethod`, etc., dispatch calls to built-in methods on specific object types.
- **setLValue (`void setLValue(ASTNode &target, QuantumValue val, const std::string &op)`):** Sets the value of an l-value target based on the operation performed.

### QuantumValue Class

**Purpose:** Represents a value in the QuantumLanguage runtime environment, capable of holding various data types including integers, doubles, strings, arrays, dictionaries, functions, and pointers.

**Trade-offs:** While providing flexibility, the use of a union-like structure (`QuantumValue`) for storing multiple data types introduces potential issues with alignment and performance optimizations.

### Environment Class

**Purpose:** Manages variable bindings and scopes, ensuring that variables are properly isolated and accessible during execution.

**Trade-offs:** Maintaining a hierarchical scope system adds complexity but provides necessary functionality for managing nested scopes and variable lifetimes.

## Tradeoffs/Limitations

- **Infinite Loop Prevention:** The step count limit prevents certain types of infinite loops but may be too restrictive for some valid programs.
- **Performance:** The use of `QuantumValue` for multi-type storage might impact performance due to alignment and memory usage considerations.
- **Complexity:** Managing environments and handling different types of expressions/statements adds complexity to the interpreter.

This README documents the core components and design decisions of the `Interpreter.h` file, providing insights into how the QuantumLanguage compiler executes and evaluates programs.