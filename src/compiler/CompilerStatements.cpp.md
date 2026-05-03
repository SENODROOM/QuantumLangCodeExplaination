# Quantum Language Compiler: Statements Compilation

## Overview

The `src/compiler/CompilerStatements.cpp` file is a crucial component of the Quantum Language compiler responsible for compiling various types of statements including variable declarations, function declarations, and class declarations. This file transforms high-level quantum language code into executable bytecode by handling different statement structures and emitting corresponding operations to the virtual machine (VM).

## Role in Compiler Pipeline

This file operates during the compilation phase of the Quantum Language compiler. It takes parsed AST nodes representing statements and converts them into VM instructions that can be executed at runtime. The primary functions include:
- Compiling variable declarations.
- Compiling function declarations.
- Compiling class declarations.

These operations are essential for setting up the execution environment, defining entities, and establishing relationships between them.

## Key Design Decisions and Why

### Scoping and Variable Declarations

The compiler handles both global and local variable declarations. For global variables, it uses `Op::DEFINE_GLOBAL`, while for local variables, it uses `Op::DEFINE_LOCAL`. If the variable is declared with the `const` keyword, it emits `Op::DEFINE_CONST`.

**Why:** This approach ensures that variables are properly scoped and initialized before use, preventing potential runtime errors related to undefined or uninitialized variables.

### Function Declarations

When compiling function declarations, the compiler generates bytecode using `Op::MAKE_FUNCTION` or `Op::MAKE_CLOSURE` depending on whether the function has upvalues. After creating the function chunk, it binds the function to its name either globally or locally based on the scope depth.

**Why:** Functions need to be defined and callable within their respective scopes. Using closures allows functions to capture variables from outer scopes, which is a fundamental feature of many programming languages.

### Class Declarations

Class declarations are handled by creating a new class object using `Op::MAKE_CLASS`. If the class inherits from another class, it emits an `Op::INHERIT` instruction. Additionally, it binds methods to the class using `Op::BIND_METHOD`.

**Why:** Classes provide a way to organize data and functionality together. Inheritance enables code reuse and polymorphism, making complex programs more manageable and efficient.

## Major Classes/Functions Overview

### `Compiler::compileVarDecl(VarDecl &s, int line)`

Compiles a variable declaration node (`VarDecl`). Handles initialization expressions and determines whether the variable should be defined globally or locally.

### `Compiler::compileFunctionDecl(FunctionDecl &s, int line)`

Compiles a function declaration node (`FunctionDecl`). Generates bytecode for the function, creates a closure if necessary, and binds the function to its name.

### `Compiler::compileClassDecl(ClassDecl &s, int line)`

Compiles a class declaration node (`ClassDecl`). Creates a new class object, handles inheritance, and binds methods to the class.

## Tradeoffs

### Memory Usage vs. Execution Speed

Using closures for functions increases memory usage due to the additional storage required for captured variables. However, it improves execution speed by allowing direct access to these variables without needing to look them up in the global scope.

### Code Readability vs. Performance

While binding methods directly to classes enhances performance by reducing lookup times, it may slightly compromise code readability. Conversely, global method bindings offer better readability but could lead to slower execution due to increased lookup times.

### Flexibility vs. Complexity

Inheriting from other classes provides flexibility and reusability but adds complexity to the compiler's implementation. Handling nested classes also increases complexity but offers powerful features like method overriding and encapsulation.

Overall, the design decisions in `src/compiler/CompilerStatements.cpp` aim to balance performance, readability, and flexibility, providing a robust foundation for the Quantum Language compiler.