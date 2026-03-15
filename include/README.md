# QuantumLanguage Compiler - Value.h

## Overview

The `include/Value.h` header file is an essential part of the QuantumLanguage compiler, focusing on defining and managing various data types and values within the language. This file provides the foundational structures necessary for the interpreter to handle expressions, statements, and function calls effectively. By leveraging C++'s type system and features like `std::variant`, it ensures that all possible value types can be encapsulated within a single unified type (`QuantumValue`). This approach simplifies the implementation of the interpreter, allowing it to process different types of data seamlessly.

## Key Design Decisions

### Use of `std::variant`
**WHY**: Choosing `std::variant` over traditional unions allows for safer type handling and avoids issues related to undefined behavior when accessing union members directly. It also enables compile-time checking of valid types, enhancing the robustness of the compiler.

### Smart Pointers (`std::shared_ptr`)
**WHY**: Utilizing smart pointers ensures automatic memory management, preventing memory leaks and dangling pointers. This is particularly important in a compiler where objects may have complex lifetimes and ownership relationships.

### Non-Owning Pointers (`std::weak_ptr`)
**WHY**: While not explicitly shown in the provided snippet, the use of `std::weak_ptr` alongside `std::shared_ptr` would allow for safe sharing of resources without creating circular references, which could otherwise lead to memory management problems.

## Class and Function Documentation

### QuantumNil
**Purpose**: Represents the null value in QuantumLanguage.
**Behaviour**: A simple struct indicating the absence of a value.

### QuantumFunction
**Purpose**: Encapsulates the definition of a user-defined function.
**Behaviour**: Contains metadata about the function, including its parameters, body, and closure environment.

### QuantumClass
**Purpose**: Represents a user-defined class in QuantumLanguage.
**Behaviour**: Not fully defined in the snippet but intended to hold information about class attributes and methods.

### QuantumInstance
**Purpose**: Represents an instance of a user-defined class.
**Behaviour**: Holds data specific to the class instance, managed through pointers.

### QuantumNativeFunc
**Purpose**: Defines the signature for native C++ functions callable from QuantumLanguage.
**Behaviour**: A type alias for `std::function`, specifying that these functions take a vector of `QuantumValue`s and return a `QuantumValue`.

### QuantumNative
**Purpose**: Encapsulates a native C++ function along with its name.
**Behaviour**: Allows the interpreter to call C++ functions directly, providing a bridge between the interpreted language and native functionality.

### QuantumValue
**Purpose**: A variant type capable of holding any of the supported QuantumLanguage data types.
**Behaviour**: Provides constructors for initializing different types of values and methods for accessing their contents safely.

## Tradeoffs and Limitations

- **Memory Overhead**: Using smart pointers incurs additional overhead compared to raw pointers, which might impact performance in scenarios with high object creation and destruction rates.
- **Complexity**: The introduction of multiple data types and smart pointers increases the complexity of the codebase, making it harder to understand and maintain.
- **Type Safety**: While `std::variant` offers compile-time type safety, it requires careful handling to avoid runtime errors due to invalid type accesses.

## Usage Examples

Here’s how you might create and manipulate `QuantumValue` instances:

```cpp
#include "Value.h"

void exampleUsage()
{
    QuantumValue nilVal; // Default constructor creates a QuantumNil value
    assert(nilVal.isNil());

    QuantumValue boolVal(true); // Bool constructor
    assert(boolVal.getBool());

    QuantumValue intVal(42); // Int constructor
    assert(intVal.getInt() == 42);

    QuantumValue strVal("Hello"); // String constructor
    assert(strVal.getString() == "Hello");

    // Creating a function value
    auto funcBody = new ASTNode(); // Assuming ASTNode is defined elsewhere
    QuantumFunction func{"myFunc", {"param1"}, {}, funcBody, nullptr};
    QuantumValue funcVal(func);
    assert(funcVal.isFunction());
}
```

This README.md provides a comprehensive overview of the `include/Value.h` file, detailing its purpose, design decisions, and usage examples. It aims to help developers understand the intricacies of managing data types in the QuantumLanguage compiler.