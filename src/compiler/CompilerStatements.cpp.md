# Quantum Language Compiler: Statements Compilation

## Overview

The `src/compiler/CompilerStatements.cpp` file plays a crucial role in the Quantum Language compiler's pipeline, focusing on the compilation of statements like variable declarations, function declarations, and class declarations. This file ensures that high-level quantum language code is converted into efficient executable bytecode.

### Key Design Decisions and Why

1. **Scope Management**: The compiler maintains a stack of scopes to manage local variables and constants. Each scope has its own depth, allowing the compiler to differentiate between global and local identifiers.
   
2. **Bytecode Emission**: The compiler uses a series of opcodes (`Op`) to generate bytecode. These opcodes represent different operations such as loading values, defining variables, and creating functions or closures.

3. **Closure Creation**: When compiling function declarations, the compiler creates closures if necessary. A closure captures the environment in which it was defined, including any upvalues (local variables from outer scopes).

4. **Inheritance Handling**: For class declarations, the compiler supports inheritance. If a base class is specified, the compiler emits instructions to load the base class and perform inheritance.

5. **Method Binding**: Class members can be methods. The compiler binds these methods to the class object during compilation, allowing them to be called later.

### Major Classes/Functions Overview

- **Compiler Class**:
  - **Role**: Manages the overall compilation process, including parsing, semantic analysis, and bytecode generation.
  - **Key Functions**:
    - `compileVarDecl`: Compiles variable declarations, handling both initialized and uninitialized cases.
    - `compileFunctionDecl`: Compiles function declarations, generating bytecode for the function body and managing closures.
    - `compileClassDecl`: Compiles class declarations, supporting inheritance and method binding.

- **VarDecl Class**:
  - **Role**: Represents a variable declaration in the source code.
  - **Key Attributes**:
    - `name`: The name of the variable.
    - `initializer`: An optional expression used to initialize the variable.
    - `isConst`: Indicates whether the variable is declared as constant.

- **FunctionDecl Class**:
  - **Role**: Represents a function declaration in the source code.
  - **Key Attributes**:
    - `name`: The name of the function.
    - `params`: A list of parameter names.
    - `paramIsRef`: Indicates whether each parameter is passed by reference.
    - `defaultArgs`: Default argument values for parameters.
    - `body`: The body of the function, represented as another AST node.

- **ClassDecl Class**:
  - **Role**: Represents a class declaration in the source code.
  - **Key Attributes**:
    - `name`: The name of the class.
    - `base`: The name of the base class, if any.
    - `members`: A list of class members (methods and fields).

### Tradeoffs

- **Complexity vs. Efficiency**: Managing multiple scopes and handling closures adds complexity to the compiler but allows for more efficient execution of quantum programs.
  
- **Memory Usage**: Storing upvalues in closures increases memory usage, but it enables capturing and reusing the environment at runtime.
  
- **Flexibility vs. Simplicity**: Supporting inheritance and method binding provides flexibility but also complicates the compiler's logic.

By understanding these components and their interactions, developers can better appreciate how the Quantum Language compiler processes and compiles statements, ultimately leading to efficient and correct quantum program execution.