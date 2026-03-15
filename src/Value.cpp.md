# Value.cpp - Core Data Representation Module of Quantum Language Compiler

## Overview

`Value.cpp` is a critical component of the Quantum Language compiler, responsible for managing and manipulating various types of values within the language's runtime environment. This module defines the `QuantumValue` class and related functions to handle these values efficiently and accurately.

## Role in the Compiler Pipeline

`Value.cpp` plays a pivotal role in the Quantum Language compiler by providing a unified way to represent and operate on different value types. It ensures type safety and provides methods to convert values into strings, check their truthiness, and perform other operations necessary during compilation and execution.

## Key Design Decisions

### Use of `std::variant` for Flexible Types

The primary design decision in `Value.cpp` is to use `std::variant` to represent different value types flexibly. This choice allows the compiler to handle multiple data types without resorting to unions or pointers, thus avoiding potential issues such as memory alignment and type safety. By leveraging `std::variant`, we can ensure that each value has a specific type at compile time, which helps catch errors early and improves performance.

### Visitor Pattern for Type-Safe Operations

To perform operations on `QuantumValue` instances in a type-safe manner, the `std::visit` function is used along with a visitor pattern. This approach ensures that the correct operation is performed based on the actual type of the value, preventing runtime errors due to incorrect type assumptions. The visitor pattern also makes it easy to extend the functionality of `QuantumValue` by adding new types and corresponding operations.

## Documentation of Major Classes and Functions

### QuantumValue Class

**Purpose:** Represents a value in the Quantum Language runtime environment. It uses `std::variant` to store different types of values.

**Behaviour:** Provides methods to check the truthiness of a value (`isTruthy()`), convert a value to a string (`toString()`), and access the underlying value.

**Tradeoffs/Limitations:** Using `std::variant` adds overhead compared to traditional union-based approaches. However, it offers better type safety and flexibility.

### isTruthy Function

**Purpose:** Determines whether a given `QuantumValue` instance is considered "truthy" in the context of conditional statements.

**Behaviour:** Returns `true` if the value is not `QuantumNil`, `bool` true, non-zero `double`, non-empty `std::string`, non-empty `Array`, non-null `QuantumPointer`, or any other non-default value. Otherwise, returns `false`.

**Tradeoffs/Limitations:** The definition of "truthy" may vary depending on the language's semantics. This implementation aims to cover common cases but might need adjustments for specific use cases.

### toString Function

**Purpose:** Converts a `QuantumValue` instance to its string representation.

**Behaviour:** Depending on the value type, it returns a stringified version of the value. For example, integers are converted to strings, doubles are formatted to a precision of 10, and arrays and dictionaries are represented in a readable format.

**Tradeoffs/Limitations:** Formatting doubles with high precision can lead to large string sizes, potentially impacting performance. Additionally, representing complex types like arrays and dictionaries requires careful handling to ensure readability and correctness.

## Conclusion

`Value.cpp` is an essential part of the Quantum Language compiler, providing a robust framework for handling and manipulating values. Its use of `std::variant` and the visitor pattern ensures type safety and flexibility, making it a valuable tool for developers working on the compiler. While there are some tradeoffs associated with its implementation, the benefits outweigh the costs, resulting in a more reliable and efficient compiler.