# QuantumLanguage Compiler - Value.h

## Overview

The `include/Value.h` header file is an essential part of the QuantumLanguage compiler, focusing on defining and managing various data types and values within the language. This file serves as the foundation for the interpreter's ability to handle expressions, statements, and function calls efficiently. By leveraging C++'s type system and features like `std::variant`, it encapsulates all possible value types into a single unified structure, ensuring flexibility and robustness in the compiler's execution phase.

## Key Design Decisions

### Use of `std::variant`
The decision to use `std::variant` was pivotal because it allows for compile-time polymorphism without sacrificing performance. Unlike dynamic casting, which incurs runtime overhead, `std::variant` provides a safer and more efficient way to manage multiple data types within a single container. This choice enhances the safety and reliability of the compiler, making it easier to reason about the types of values being handled at any given point.

### Non-Owning Pointers for Function Bodies
By using non-owning pointers (`std::weak_ptr`) for function bodies instead of owning pointers (`std::shared_ptr`), we optimize memory usage. Non-owning pointers do not increase the reference count of the function body, thus preventing unnecessary memory allocation when functions are defined but not immediately invoked. This approach balances memory efficiency with the need to access function bodies during interpretation.

## Documentation of Major Classes/Functions

### QuantumNil
**Purpose**: Represents the `nil` value in QuantumLanguage, similar to `null` in other languages.
**Behaviour**: Acts as a placeholder for empty or uninitialized values. No specific operations are defined since `nil` is essentially a marker.

### QuantumFunction
**Purpose**: Encapsulates information about a user-defined function in QuantumLanguage.
**Behaviour**: Contains details such as the function's name, parameters, default arguments, and the body of the function. The `closure` member holds a shared pointer to the environment in which the function was defined, allowing it to maintain state across invocations.

### QuantumPointer
**Purpose**: Represents a pointer to a variable in QuantumLanguage.
**Behaviour**: Holds a shared pointer to the actual variable storage (`cell`) and additional metadata like the variable's name (`varName`) and an offset for pointer arithmetic. Provides methods to check if the pointer is null and to dereference the pointer safely.

### QuantumValue
**Purpose**: A versatile structure capable of holding any valid value in QuantumLanguage.
**Behaviour**: Utilizes `std::variant` to store different types of values, including basic types (`bool`, `double`, `std::string`), arrays, dictionaries, functions, native functions, instances, classes, and pointers. Offers constructors for initializing various types of values and methods to retrieve their contents safely.

## Tradeoffs/Limitations

- **Memory Efficiency**: Using non-owning pointers for function bodies optimizes memory usage but requires careful management to avoid dangling references.
- **Type Safety**: While `std::variant` provides strong type safety, handling errors related to invalid type accesses (e.g., dereferencing a null pointer) necessitates robust exception handling mechanisms.
- **Performance**: Although `std::variant` offers compile-time polymorphism, accessing variant members can still introduce some runtime overhead compared to traditional unions.

These considerations reflect the balance between functionality, performance, and resource utilization that characterizes the design of the `Value.h` header file in the QuantumLanguage compiler.