# QuantumLanguage Compiler - Interpreter.h

## Overview

The `include/Interpreter.h` header file is an essential part of the QuantumLanguage compiler, focusing on interpreting and executing abstract syntax trees (ASTs). It provides a robust framework for handling different types of AST nodes, including variable declarations, function calls, control structures, and more. The primary goal of this file is to translate the syntactic representation of code into executable operations, ensuring that the language's semantics are accurately interpreted.

## Role in Compiler Pipeline

The `Interpreter.h` file plays a critical role in the compiler pipeline by serving as the bridge between the parsing phase and the execution phase. After the source code is parsed into an AST, the interpreter takes over to evaluate and execute the program according to its defined rules. This separation allows the compiler to focus on generating efficient intermediate representations without worrying about the execution details.

## Key Design Decisions and Why

1. **Separation of Concerns**: The interpreter is designed with a clear separation between statement execution and expression evaluation. This modular approach simplifies the implementation and makes it easier to extend or modify specific parts of the interpreter.

2. **Environment Management**: The use of environments (`std::shared_ptr<Environment>`) facilitates the management of variables and their scopes. Each environment represents a level of scope, allowing for nested environments and dynamic scoping mechanisms.

3. **Built-in Method Dispatch**: To support object-oriented features like method calls, the interpreter includes specialized functions for calling built-in methods on different types of objects (arrays, strings, dictionaries). This ensures that the correct method is invoked based on the object's type and the method name provided.

4. **Infinite Loop Protection**: The `stepCount_` mechanism guards against potential infinite loops, such as those caused by empty input programs. This feature helps maintain the stability and reliability of the interpreter.

## Major Classes/Functions Overview

### `Interpreter` Class

- **Constructor**: Initializes the interpreter with global and local environments.
- **execute(ASTNode &node)**: Executes the given AST node.
- **evaluate(ASTNode &node)**: Evaluates the value of the given AST node.
- **execBlock(BlockStmt &s, std::shared_ptr<Environment> scope = nullptr)**: Executes a block of statements within a specified scope.
- **registerNatives()**: Registers native functions that can be called from the quantum language.
- **Statement Executors**: Functions like `execVarDecl`, `execFunctionDecl`, etc., handle the execution of different types of AST nodes.
- **Expression Evaluators**: Functions like `evalBinary`, `evalUnary`, etc., evaluate the value of different types of expressions.

### `QuantumValue` Class

Represents the value of an expression in the quantum language. It encapsulates various data types and provides methods for arithmetic operations, comparisons, and conversions.

### `callFunction`, `callNative`, `callInstanceMethod`

These functions handle the invocation of user-defined, native, and instance methods, respectively. They ensure that the correct method is called with the appropriate arguments and return values.

### `callMethod`, `callArrayMethod`, `callStringMethod`, `callDictMethod`

Specialized methods for invoking built-in methods on arrays, strings, and dictionaries. These functions provide a consistent interface for method calls across different object types.

### `setLValue`

Handles setting the value of left-hand-side expressions, supporting both simple assignments and compound assignment operators.

## Tradeoffs

1. **Performance vs. Flexibility**: While the separation of concerns enhances flexibility, it may introduce some performance overhead due to additional function calls and checks.

2. **Memory Usage**: Managing multiple environments requires careful memory allocation and deallocation to avoid memory leaks and excessive memory usage.

3. **Complexity**: The addition of built-in method dispatch increases the complexity of the interpreter, making it harder to understand and maintain.

Overall, the `Interpreter.h` file is a vital component of the QuantumLanguage compiler, providing a comprehensive and flexible framework for interpreting and executing the language's constructs. Its design decisions balance performance, flexibility, and complexity, ensuring that the interpreter remains efficient and reliable while supporting advanced language features.