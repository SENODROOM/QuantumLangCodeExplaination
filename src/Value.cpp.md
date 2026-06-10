# Value.cpp - Core Data Representation Module of Quantum Language Compiler

## Overview

`Value.cpp` is a crucial component of the Quantum Language compiler, responsible for managing and manipulating various data types throughout the compilation process. The primary focus is on the `QuantumValue` class, which encapsulates fundamental data types like nil, booleans, doubles, strings, arrays, pointers, dictionaries, closures, native functions, and instances. This module ensures that all data values are handled consistently and efficiently during the compilation stages.

### Role in Compiler Pipeline

- **Data Storage**: Stores intermediate and final results of expressions and operations.
- **Type Handling**: Manages type conversions and checks to ensure type safety.
- **Evaluation**: Evaluates expressions based on their data types.
- **Serialization**: Converts data values into string representations for debugging and output.

## Key Design Decisions and Why

1. **Use of `std::variant`**:
   - **Why**: To support multiple data types within a single value object without resorting to unions or inheritance, ensuring type safety and flexibility.
   
2. **Visitor Pattern**:
   - **Why**: To handle different data types polymorphically, allowing for clean and extensible code. The visitor pattern simplifies the implementation of operations across different data types.

3. **Smart Pointers (`std::shared_ptr`) for Complex Types**:
   - **Why**: To manage memory automatically for complex data structures like arrays, dictionaries, and closures, preventing memory leaks and dangling references.

4. **Custom String Conversion**:
   - **Why**: To provide precise control over how each data type is represented as a string, ensuring readability and consistency in error messages and outputs.

## Major Classes/Functions Overview

### QuantumValue Class

The `QuantumValue` class serves as the base class for all data types in the Quantum Language. It uses `std::variant` to store different data types internally. The class provides methods to check if a value is truthy, convert it to a string representation, and perform other common operations.

#### Methods

- **isTruthy()**: Determines if the value is considered "truthy" in the context of conditional statements.
- **toString()**: Converts the value to its string representation, handling special cases like integers, floating-point numbers, and complex objects.

### Array Class

The `Array` class represents an ordered collection of values. It uses `std::vector` to store elements and provides methods to access, modify, and iterate over them.

### Dict Class

The `Dict` class represents an unordered collection of key-value pairs. It uses `std::unordered_map` to store elements and provides methods to add, remove, and retrieve values based on keys.

### Closure Class

The `Closure` class represents a function with captured variables. It stores the name of the function and a reference to the environment in which it was defined.

### Native Function Class

The `NativeFunction` class represents a function implemented in C++. It stores the name of the function and a pointer to the underlying C++ function.

### Instance Class

The `Instance` class represents an object instance of a user-defined class. It stores a reference to the class definition and a map of property values.

## Tradeoffs

- **Memory Usage**: Using smart pointers can lead to increased memory overhead due to dynamic allocation and deallocation.
- **Performance**: Visitor pattern can introduce some performance overhead compared to direct method calls, but it enhances code maintainability and scalability.
- **Complexity**: The use of `std::variant` and smart pointers adds complexity to the code, requiring careful management of types and memory.

Overall, `Value.cpp` plays a pivotal role in the Quantum Language compiler by providing a robust framework for handling various data types. Its design choices, including the use of `std::variant`, visitor pattern, and smart pointers, offer a balance between type safety, flexibility, and performance.