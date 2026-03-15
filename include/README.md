# QuantumLanguage Compiler - Value.h

## Overview

The `include/Value.h` header file is a crucial component of the QuantumLanguage compiler, responsible for defining and managing various data types and values within the language. It serves as the foundation for the interpreter's ability to handle expressions, statements, and function calls efficiently. The file includes definitions for basic value types such as integers, doubles, strings, arrays, dictionaries, functions, native functions, instances, classes, and pointers, all encapsulated within a single `QuantumValue` variant structure.

## Key Design Decisions

### Use of `std::variant`
The decision to use `std::variant` over traditional unions was driven by the need for type safety and ease of handling multiple data types dynamically. `std::variant` provides compile-time type checking and allows for direct access to the contained type without manual type casting, reducing runtime errors and improving code readability.

### Smart Pointers (`std::shared_ptr`)
Smart pointers were chosen to manage memory automatically and prevent memory leaks. Specifically, `std::shared_ptr` was used for storing complex objects like arrays, dictionaries, functions, and instances, ensuring that these objects are properly deallocated when they are no longer needed. This approach simplifies memory management and reduces the risk of dangling pointers.

### Non-Owning Pointers
For certain types of values, such as function bodies (`ASTNode* body` in `QuantumFunction`), non-owning pointers (`std::weak_ptr`) could have been used instead of shared pointers. However, since the function bodies are owned by their respective nodes in the abstract syntax tree (AST), using `std::shared_ptr` ensures that the function bodies remain valid as long as the AST nodes are alive.

## Documentation of Major Classes/Functions

### QuantumNil
**Purpose:** Represents the "nil" or "null" value in QuantumLanguage.
**Behavior:** A simple struct with no members, indicating an empty or undefined state.

### QuantumFunction
**Purpose:** Encapsulates a user-defined function in QuantumLanguage.
**Behavior:** Contains metadata about the function, including its name, parameters, default arguments, body, and closure environment. The function body is stored as a non-owning pointer to an `ASTNode`.

### QuantumClass
**Purpose:** Represents a user-defined class in QuantumLanguage.
**Behavior:** Not fully defined in the snippet but likely contains information about class attributes, methods, and inheritance relationships.

### QuantumInstance
**Purpose:** Represents an instance of a user-defined class.
**Behavior:** Not fully defined in the snippet but likely contains data specific to the instance, such as attribute values.

### QuantumNativeFunc
**Purpose:** Defines a signature for native functions in QuantumLanguage.
**Behavior:** A type alias for a function that takes a vector of `QuantumValue` objects and returns a `QuantumValue`. This allows native functions to be integrated seamlessly into the language's evaluation process.

### QuantumNative
**Purpose:** Encapsulates a native function in QuantumLanguage.
**Behavior:** Stores the name of the native function and its implementation as a `QuantumNativeFunc`.

### QuantumPointer
**Purpose:** Represents a pointer to a variable in QuantumLanguage.
**Behavior:** Contains a shared pointer to the variable's storage location (`QuantumValue`), the variable's name, and an offset for pointer arithmetic. Provides methods to check if the pointer is null and to dereference it safely.

### QuantumValue
**Purpose:** A variant structure that can hold any of the supported data types in QuantumLanguage.
**Behavior:** Contains a `Data` member, which is a `std::variant` capable of holding instances of `QuantumNil`, `bool`, `double`, `std::string`, `Array`, `Dict`, `QuantumFunction`, `QuantumNative`, `QuantumInstance`, `QuantumClass`, and `QuantumPointer`. Offers constructors for initializing `QuantumValue` objects with different data types.

## Tradeoffs/Limitations

- **Performance Overhead:** Using `std::variant` and smart pointers introduces some performance overhead compared to traditional unions and raw pointers. However, this tradeoff is deemed acceptable given the benefits of type safety and automatic memory management.
- **Complexity:** The introduction of multiple data types and smart pointers increases the complexity of the codebase. Developers must be aware of the potential pitfalls associated with dynamic typing and memory management.
- **Memory Usage:** While smart pointers help prevent memory leaks, they also increase memory usage due to the overhead of reference counting.

By carefully considering these factors, the QuantumLanguage compiler aims to provide a powerful yet safe and efficient environment for developers to create and execute quantum programs.