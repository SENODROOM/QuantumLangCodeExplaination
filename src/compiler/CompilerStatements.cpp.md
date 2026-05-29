# Quantum Language Compiler: Statements Compilation

## Overview

The `src/compiler/CompilerStatements.cpp` file is an integral part of the Quantum Language compiler's pipeline, responsible for the compilation of various statement types such as variable declarations, function declarations, and class declarations. This file ensures that high-level quantum language code is efficiently transformed into executable bytecode.

### Key Design Decisions and Why

1. **Scope Management**: The compiler maintains a scope depth to track where variables are declared. Variables declared at the global level are defined using `Op::DEFINE_GLOBAL`, while those within functions or local scopes use `Op::DEFINE_LOCAL`. This decision ensures that variables are correctly accessible during runtime.

2. **Initialization Handling**: When compiling variable declarations, the compiler checks if an initializer is provided. If so, it compiles the initializer expression; otherwise, it emits a `LOAD_NIL` operation to initialize the variable with `nil`. This approach simplifies handling uninitialized variables without introducing unnecessary complexity.

3. **Closure Creation**: For function declarations, the compiler creates closures when necessary. A closure is emitted using `Op::MAKE_CLOSURE` if there are upvalues, otherwise `Op::MAKE_FUNCTION`. This allows functions to capture outer variables, which is essential for functional programming patterns.

4. **Inheritance Support**: In class declarations, the compiler supports inheritance by emitting an `INHERIT` operation after creating the base class chunk. This enables derived classes to inherit properties and methods from their parent classes.

5. **Method Binding**: The compiler binds methods to the class during compilation. It handles different cases such as nested class declarations, field initializations, and assignment expressions. This ensures that all methods are properly associated with their respective classes.

## Major Classes/Functions Overview

- **Compiler Class**: Manages the overall compilation process, including managing the symbol table and scope stack. It contains methods to compile individual statement types (`compileVarDecl`, `compileFunctionDecl`, `compileClassDecl`).

- **compileVarDecl Function**: Compiles variable declarations. It handles both initialized and uninitialized variables, ensuring they are correctly defined at their respective scopes.

- **compileFunctionDecl Function**: Compiles function declarations. It generates bytecode for the function body, creates closures if necessary, and defines the function globally or locally based on its scope.

- **compileClassDecl Function**: Compiles class declarations. It manages class creation, inheritance, and method binding. Nested class declarations and field initializations are also handled within this function.

## Tradeoffs

1. **Memory Usage**: Maintaining a symbol table and scope stack can increase memory usage, especially for large programs. However, this overhead is necessary for accurate scope management and variable resolution.

2. **Complexity**: Supporting closures and inheritance adds complexity to the compiler. While these features are powerful, they require careful handling to ensure correct bytecode generation and execution.

3. **Performance**: Efficiently managing scopes and generating bytecode requires optimized algorithms. Balancing performance with correctness and simplicity is a constant challenge in compiler design.

Overall, the `src/compiler/CompilerStatements.cpp` file provides a robust foundation for compiling quantum language statements, balancing functionality, accuracy, and performance.