# Value.cpp - Core Data Representation Module of Quantum Language Compiler

## Overview

`Value.cpp` is a fundamental module in the Quantum Language compiler, designed to manage and manipulate various types of values within the language's runtime environment. This module introduces the `QuantumValue` class and associated functions, ensuring efficient and accurate handling of these values across different stages of compilation.

## Role in the Compiler Pipeline

`Value.cpp` serves as a crucial layer in the Quantum Language compiler's architecture. It encapsulates the value representation and provides methods to check truthiness and convert values to strings. These functionalities are essential for evaluating expressions, controlling program flow, and generating output during the compilation process.

## Key Design Decisions

### Use of `std::variant` for Value Storage

The decision to use `std::variant` for storing different types of values was driven by the need for type safety and flexibility. By leveraging `std::variant`, we can safely store and manipulate multiple data types without resorting to unions or raw pointers, reducing the risk of undefined behavior and memory corruption.

### Visitor Pattern for Value Operations

Implementing the visitor pattern allows us to define a set of operations on `QuantumValue` objects without modifying their structure. This approach promotes code modularity and extensibility, enabling easy addition of new value types and corresponding operations in the future.

## Documentation of Major Classes and Functions

### QuantumValue Class

**Purpose:** The `QuantumValue` class represents a value in the Quantum Language runtime environment. It uses `std::variant` to store different types of values, such as integers, booleans, strings, arrays, dictionaries, functions, and native objects.

**Behavior:** 
- `isTruthy()` checks whether the value is considered "truthy" in the context of conditional statements.
- `toString()` converts the value to its string representation, which is useful for debugging and error messages.

### isTruthy Function

**Purpose:** Determines if a `QuantumValue` is considered "truthy." A value is deemed falsy if it is `QuantumNil`, `false`, or an empty array/string/dictionary.

**Behavior:** Uses `std::visit` to apply the appropriate lambda function based on the value's type, returning `true` for all non-falsy values.

### toString Function

**Purpose:** Converts a `QuantumValue` to its string representation.

**Behavior:** Utilizes `std::visit` to handle different value types:
- `QuantumNil`: Returns `"nil"`
- `bool`: Returns `"true"` or `"false"`
- `double`: Formats the number to a string, handling large integers specially
- `std::string`: Returns the string itself
- `std::shared_ptr<Array>`: Generates a string representation of the array, including elements
- `std::shared_ptr<Dict>`: Creates a string representation of the dictionary, including key-value pairs
- `std::shared_ptr<QuantumFunction>`: Returns a string indicating the function name
- `std::shared_ptr<QuantumNative>`: Returns a string indicating the native object name
- `std::shared_ptr<QuantumInstance>`: Calls the `__str__` method if defined, otherwise returns a default string representation

## Tradeoffs and Limitations

### Type Safety vs. Memory Overhead

Using `std::variant` ensures type safety but may introduce some memory overhead compared to traditional union-based approaches. However, this tradeoff is necessary for maintaining robustness and preventing runtime errors due to incorrect type usage.

### Performance Considerations

While the visitor pattern offers flexibility and extensibility, it may impact performance due to the overhead of virtual function calls. Future optimizations could explore alternative patterns or techniques to mitigate this issue.

### Limited Support for Complex Types

Currently, the implementation supports basic types like integers, booleans, and strings. More complex types, such as custom classes or nested structures, require additional support and may not be fully implemented yet.

## Conclusion

`Value.cpp` plays a pivotal role in the Quantum Language compiler by providing a unified and type-safe way to represent and manipulate values. Its design choices, particularly the use of `std::variant` and the visitor pattern, offer significant benefits in terms of flexibility and correctness. While there are some limitations and areas for improvement, the core functionality remains solid and forms the foundation for further development and enhancements in the compiler.