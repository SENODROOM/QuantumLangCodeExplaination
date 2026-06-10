# QuantumLanguage Compiler - Value.h

## Overview

The `include/Value.h` header file is an integral part of the QuantumLanguage compiler, focusing on the representation and management of values within the system. This file defines various types of quantum values and their associated operations, ensuring efficient execution and accurate program behavior.

## Role in Compiler Pipeline

In the QuantumLanguage compiler's pipeline, `Value.h` serves as a foundational component. It encapsulates all the different types of values that can be manipulated during the compilation and execution phases. By providing a unified interface for these values, it simplifies the interaction between different parts of the compiler, such as the parser, interpreter, and optimizer.

## Key Design Decisions and Why

1. **Use of Variants**: The primary design decision in `Value.h` is the use of `std::variant` to represent different types of quantum values. This choice allows for a type-safe way to store and manipulate multiple value types without resorting to unions or polymorphism, which can lead to more readable and maintainable code.

2. **Shared Pointers**: To manage memory efficiently and avoid dangling pointers, `QuantumValue` uses shared pointers (`std::shared_ptr`). This ensures that any dynamically allocated memory is properly managed and deallocated when no longer needed, preventing memory leaks and other related issues.

3. **Custom Types**: Beyond basic types like integers and strings, `Value.h` introduces custom types such as closures, instances, and classes. These types are essential for supporting higher-order functions, object-oriented programming, and other advanced features of the QuantumLanguage.

4. **Exception Handling**: The inclusion of exception handling mechanisms, particularly through the use of `std::runtime_error`, helps ensure robustness and reliability of the compiler. Proper error handling prevents crashes and provides meaningful feedback to developers.

## Major Classes/Functions Overview

### QuantumValue Class

- **Purpose**: Represents a quantum value, which can be one of several different types including booleans, numbers, strings, arrays, dictionaries, and custom objects.
- **Key Features**:
  - Uses `std::variant` to store different value types.
  - Provides constructors for each supported value type.
  - Includes methods to check the type of the stored value and retrieve its content.

### QuantumPointer Struct

- **Purpose**: Represents a pointer to a quantum value, allowing for dynamic memory management and pointer arithmetic.
- **Key Features**:
  - Contains a `std::shared_ptr` to the actual value, ensuring proper memory management.
  - Stores the variable name and offset for debugging purposes.
  - Implements methods to check if the pointer is null and to dereference the pointer safely.

### QuantumNativeFunc and QuantumNative Structs

- **Purpose**: Represent native functions that can be called from within the quantum language.
- **Key Features**:
  - `QuantumNativeFunc` is a function pointer that takes a vector of `QuantumValue`s and returns a `QuantumValue`.
  - `QuantumNative` stores the name and function pointer of a native function, making it easy to look up and invoke.

## Tradeoffs

1. **Type Safety vs. Performance**: While `std::variant` provides strong type safety, it may introduce some performance overhead compared to simpler data structures. However, the benefits of safer code outweigh this cost in most cases.

2. **Memory Management**: Using shared pointers simplifies memory management but can also increase the complexity of the code due to reference counting and potential memory contention.

3. **Flexibility vs. Complexity**: Introducing custom types like closures and classes increases flexibility but adds complexity to the implementation and usage of these types.

Overall, `Value.h` is a critical component of the QuantumLanguage compiler, designed to handle a wide range of value types efficiently and safely. Its use of modern C++ features like `std::variant` and `std::shared_ptr` demonstrates a commitment to both functionality and performance.