# Quantum Language Compiler: Statements Compilation

## Overview

The `src/compiler/CompilerStatements.cpp` file is a critical component of the Quantum Language compiler responsible for compiling various types of statements such as variable declarations, function declarations, and class declarations. This file plays a pivotal role in transforming high-level quantum language code into executable bytecode that can be run on the Quantum Virtual Machine (QVM).

## Key Design Decisions and Why

### Scope Management

- **Global vs Local Variables**: The compiler distinguishes between global and local variables based on the scope depth. For global variables, it uses `Op::DEFINE_GLOBAL`, while for local variables, it uses `Op::DEFINE_LOCAL`. This decision ensures proper encapsulation and prevents naming conflicts within nested scopes.

### Closure Creation

- **Function Declarations**: When compiling function declarations, the compiler creates closures using `Op::MAKE_CLOSURE` or `Op::MAKE_FUNCTION`. This allows functions to capture and use variables from their enclosing environments, which is essential for functional programming features like higher-order functions and closures.

### Method Binding

- **Class Declarations**: In class declarations, each method is compiled separately, and then bound to the class using `Op::BIND_METHOD`. This approach ensures that methods are correctly associated with their respective classes and instances, facilitating object-oriented programming.

## Major Classes/Functions Overview

### `Compiler`

- **Role**: The primary class responsible for compiling quantum language code into bytecode.
- **Key Functions**:
  - `compileVarDecl`: Compiles variable declarations, handling both initialized and uninitialized variables.
  - `compileFunctionDecl`: Compiles function declarations, creating closures and binding them to the appropriate scope.
  - `compileClassDecl`: Compiles class declarations, including inheritance and method binding.

### `Closure`

- **Role**: Represents a function along with its environment (closure).
- **Key Features**:
  - Captures variables from the enclosing scope.
  - Allows functions to maintain state across multiple invocations.

### `Op`

- **Role**: Enumerates the different bytecode operations supported by the QVM.
- **Key Operations**:
  - `OP_DEFINE_CONST`: Defines a constant in the global or local scope.
  - `OP_MAKE_CLOSURE`: Creates a closure for a function.
  - `OP_BIND_METHOD`: Binds a method to a class.

## Tradeoffs

- **Memory Usage**: Creating closures can lead to increased memory usage due to captured variables. However, this is necessary for maintaining state and supporting advanced programming paradigms.
- **Performance**: While closures provide flexibility, they may introduce performance overhead compared to simple functions. Optimizations and careful management of upvalues are crucial to mitigate these effects.
- **Complexity**: Handling different types of declarations (variables, functions, classes) adds complexity to the compiler. Ensuring correct scoping, capturing, and binding requires meticulous implementation and testing.

This README provides an overview of the `src/compiler/CompilerStatements.cpp` file's role in the Quantum Language compiler, key design decisions, major components, and potential tradeoffs. It serves as a guide for developers working on extending or modifying the compiler's functionality.