# Quantum Language Compiler: `src/compiler/CompilerFunctions.cpp`

## Role in Compiler Pipeline

`CompilerFunctions.cpp` plays a critical role in the Quantum Language compiler's pipeline by handling the compilation of function definitions into executable chunks. Its primary responsibilities include:

- Creating new scopes to manage variable declarations within functions.
- Declaring local variables based on function parameters.
- Compiling the function body to generate executable instructions.
- Ensuring proper scope management and variable lifetime tracking during compilation.

This component is essential for transforming high-level quantum language constructs into low-level machine code that can be executed by the Quantum Virtual Machine (QVM).

## Key Design Decisions and WHY

### Scope Management

**Decision:** To maintain separate scope states for function declarations.

**Why:** Each function has its own set of local variables, which must not interfere with global or other function scopes. By maintaining separate scope states (`CompilerState`), we ensure that variable bindings are correctly resolved at runtime.

### Parameter Handling

**Decision:** To support both regular and reference parameters.

**Why:** Quantum programming often requires mutable operations on data. By supporting reference parameters, the compiler can pass references to local variables, allowing modifications to persist across different parts of the program.

### Error Handling

**Decision:** To use exceptions for error reporting.

**Why:** Exceptions provide a clean and structured way to handle errors during compilation. They allow for centralized error management and easier debugging compared to manual error checking and reporting mechanisms.

### Tradeoffs

- **Memory Usage:** Maintaining multiple scope states consumes additional memory, but it ensures correct variable resolution and prevents potential bugs related to scope conflicts.
- **Performance:** Exception handling can introduce some overhead, especially in performance-critical applications. However, in a compiler context, where error handling is infrequent, this tradeoff is acceptable for better code clarity and maintainability.

## Major Classes/Functions Overview

### `CompilerState`

**Overview:** Represents the state of the compiler for a specific scope (function or block). It includes information about local variables, upvalues, and whether the current scope is a function.

**Key Features:**
- `locals`: A map of local variable names to their indices.
- `upvalues`: A list of upvalues used by the current scope.
- `isFunction`: A boolean indicating whether the current scope is a function.

### `compileFunction`

**Overview:** The main function for compiling a quantum function definition. It initializes a new scope, declares local variables, compiles the function body, and generates the final chunk of executable code.

**Parameters:**
- `name`: The name of the function being compiled.
- `params`: A vector of parameter names.
- `paramIsRef`: A vector indicating whether each parameter is passed by reference.
- `body`: An AST node representing the function body.
- `line`: The line number where the function definition starts.

**Key Steps:**
1. Initializes a new `CompilerState` for the function.
2. Begins a new scope and declares local variables for each parameter.
3. Compiles the function body using either `compileBlock` or `compileExpr`.
4. Emits necessary instructions to handle function returns.
5. Ends the scope and packs upvalue descriptors as constants for the closure operation.

## Conclusion

`CompilerFunctions.cpp` is a vital part of the Quantum Language compiler, ensuring that function definitions are correctly transformed into executable code. Through careful design and implementation, it provides robust scope management, flexible parameter handling, and efficient error reporting, making it an integral component of the overall compiler architecture.