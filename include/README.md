# QuantumLanguage Compiler - Value.h

## Overview

The `include/Value.h` header file is a fundamental part of the QuantumLanguage compiler, serving as the backbone for representing various data types and values within the language. This file defines a comprehensive set of structures and types that encapsulate different kinds of values, including basic types like integers, doubles, strings, arrays, dictionaries, functions, native functions, instances, classes, and pointers. By providing a robust framework for handling these values, the compiler ensures accurate evaluation and manipulation of expressions throughout the compilation process.

## Key Design Decisions

### Use of `std::variant`
The primary reason for choosing `std::variant` over traditional unions is its type safety and ease of use. `std::variant` allows for multiple possible types within a single variable, ensuring that each value has a well-defined type at compile time. This approach eliminates the risk of undefined behavior due to incorrect type access and simplifies the implementation of type-specific operations.

### Smart Pointers (`std::shared_ptr`)
Smart pointers, specifically `std::shared_ptr`, are used extensively to manage memory and ensure proper resource deallocation. By employing smart pointers, the compiler avoids manual memory management issues such as dangling pointers and memory leaks. Additionally, `std::shared_ptr` facilitates easy sharing of resources between different parts of the compiler, enhancing modularity and reusability.

### Non-Owning Pointers (`std::weak_ptr`)
In scenarios where a value needs to refer to another value without taking ownership, `std::weak_ptr` is utilized. This helps prevent circular references and memory leaks while still allowing for efficient sharing of resources. The use of `std::weak_ptr` ensures that the referenced object remains valid as long as there is an owning `std::shared_ptr`.

## Class Documentation

### QuantumNil
**Purpose**: Represents the null value in QuantumLanguage.
**Behavior**: A simple struct indicating the absence of a value.

### QuantumFunction
**Purpose**: Encapsulates information about a function defined in QuantumLanguage.
**Behavior**: Contains details such as the function's name, parameters, default arguments, body, and closure environment.

#### Tradeoffs/Limitations:
- Does not support variadic arguments directly.
- Limited flexibility in parameter passing modes beyond pass-by-value and pass-by-reference.

### QuantumClass
**Purpose**: Represents a class definition in QuantumLanguage.
**Behavior**: Not fully implemented in the provided snippet but intended to store class metadata and methods.

### QuantumInstance
**Purpose**: Represents an instance of a class in QuantumLanguage.
**Behavior**: Stores attributes and methods associated with a particular class instance.

### QuantumNativeFunc
**Purpose**: A function type alias for native functions in QuantumLanguage.
**Behavior**: Defines a callable entity that takes a vector of `QuantumValue`s and returns a `QuantumValue`.

### QuantumNative
**Purpose**: Encapsulates information about a native function in QuantumLanguage.
**Behavior**: Holds the function's name and its callable entity.

### QuantumValue
**Purpose**: A versatile structure capable of holding any value type supported by QuantumLanguage.
**Behavior**: Utilizes `std::variant` to store different types of values, including custom types like functions, instances, and classes. Provides constructors for initializing various value types and a method for accessing the stored value.

#### Tradeoffs/Limitations:
- Performance overhead associated with `std::variant`.
- Complexity in implementing type-specific operations.

## Function Documentation

No specific functions are documented in the provided snippet. However, it includes constructors for the `QuantumValue` struct, which facilitate creating instances of `QuantumValue` with different underlying types.

## Conclusion

The `include/Value.h` header file plays a critical role in the QuantumLanguage compiler by providing a unified framework for representing various value types. Through strategic design choices such as the use of `std::variant` and smart pointers, the compiler ensures both type safety and efficient memory management. While some limitations exist, particularly regarding performance and complexity, the overall architecture provides a solid foundation for further development and expansion of the QuantumLanguage compiler.