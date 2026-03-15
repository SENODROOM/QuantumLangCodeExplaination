# QuantumLanguage Compiler - Value.h

## Overview

The `include/Value.h` header file is a critical component of the QuantumLanguage compiler's architecture. It encapsulates the definition of various data structures and types essential for representing values within the language. These values include basic types such as integers, doubles, strings, arrays, dictionaries, functions, native functions, instances, classes, and pointers. This file facilitates the management of value semantics throughout the compilation process, ensuring efficient and accurate handling of data at different stages.

## Key Design Decisions

### Use of `std::variant`
**Why:** The use of `std::variant` allows for a flexible representation of multiple value types without resorting to unions or inheritance. This choice enhances type safety and readability while providing a clear and concise way to handle different value types within a single structure.

### Non-Owning Pointers for Function Bodies
**Why:** By using non-owning pointers (`std::weak_ptr`) for function bodies, the compiler avoids unnecessary memory allocations and reduces overhead. This approach ensures that the function body can be shared among multiple instances without causing ownership issues or memory leaks.

### Shared Ownership for Complex Types
**Why:** For complex types such as arrays, dictionaries, functions, native functions, instances, classes, and pointers, the compiler uses `std::shared_ptr`. This ensures that these objects can be safely shared and managed across different parts of the compiler without worrying about premature deallocation or dangling references.

## Detailed Documentation

### QuantumNil
**Purpose:** Represents the `nil` value in QuantumLanguage.
**Behaviour:** This struct is empty and serves as a placeholder for the `nil` value.

### QuantumFunction
**Purpose:** Represents a user-defined function in QuantumLanguage.
**Behaviour:** Contains information about the function, including its name, parameters, default arguments, body, and closure environment. Parameters can be passed by value or reference, indicated by the `paramIsRef` vector.

### QuantumNativeFunc
**Purpose:** A function type alias for native functions.
**Behaviour:** Defines a callable object that takes a vector of `QuantumValue`s and returns a `QuantumValue`.

### QuantumNative
**Purpose:** Represents a native function in QuantumLanguage.
**Behaviour:** Holds the name of the native function and its corresponding callable object (`QuantumNativeFunc`). Native functions are those implemented in C++ rather than QuantumLanguage itself.

### QuantumValue
**Purpose:** The primary data structure for representing values in QuantumLanguage.
**Behaviour:** Utilizes `std::variant` to store one of several possible value types, including `QuantumNil`, booleans, doubles, strings, arrays, dictionaries, functions, native functions, instances, classes, and pointers. Each constructor initializes the variant with a specific value type.

### QuantumPointer
**Purpose:** Represents a pointer to a variable in QuantumLanguage.
**Behaviour:** Contains a shared pointer to the variable storage (`QuantumPointer`), the name of the variable, and an optional offset for pointer arithmetic. Provides methods for checking if the pointer is null and dereferencing it to access the underlying variable.

## Tradeoffs and Limitations

### Memory Management
While `std::shared_ptr` provides robust memory management, it may lead to increased memory usage due to reference counting. Additionally, non-owning pointers (`std::weak_ptr`) require careful handling to avoid dangling references.

### Performance
Using `std::variant` can introduce some performance overhead compared to traditional union types. However, this is mitigated by modern compilers' optimizations and the flexibility gained from being able to represent multiple types within a single structure.

### Complexity
The introduction of multiple value types and their associated constructors can increase the complexity of the codebase. However, this complexity is necessary to provide a comprehensive and flexible representation of values within the QuantumLanguage compiler.

By understanding these details, developers can better appreciate the design choices behind the `Value.h` header file and how they contribute to the overall functionality and efficiency of the QuantumLanguage compiler.