# Value.cpp - Core Data Representation Module of Quantum Language Compiler

## Overview

`Value.cpp` is a crucial component of the Quantum Language compiler, responsible for managing and manipulating various data types encountered throughout the compilation process. This module introduces the `QuantumValue` class along with associated utility functions, ensuring efficient and accurate handling of these data types.

### Role in Compiler Pipeline

The `Value.cpp` module plays a vital role in the Quantum Language compiler's pipeline, as it forms the basis for representing and manipulating values at runtime. It facilitates the conversion between different data types and provides methods to check their truthiness and convert them to strings, which are essential for debugging and error reporting.

### Key Design Decisions and Why

1. **Use of `std::variant`**: The `QuantumValue` class uses `std::variant` to store different data types such as `QuantumNil`, `bool`, `double`, `std::string`, `Array`, `QuantumPointer`, `QuantumFunction`, `QuantumNative`, and `QuantumInstance`. This decision allows for a flexible and type-safe representation of values without the need for multiple inheritance or complex type hierarchies.

2. **Visitor Pattern**: The `isTruthy()` and `toString()` methods utilize the visitor pattern to handle each data type individually. This approach ensures that each type can define its own behavior for these operations, promoting code reuse and maintainability.

3. **Efficient String Conversion**: The `toString()` method includes optimizations for converting numeric values to strings. For integers, it directly converts to `long long` if they fit within the range, avoiding unnecessary floating-point conversions. For doubles, it formats the value to 10 decimal places, ensuring precision while keeping the string representation concise.

4. **Handling Complex Types**: The module provides specialized handling for complex types like `Array` and `Dict`. For arrays, it constructs a string representation that includes all elements, formatting strings appropriately. For dictionaries, it creates a JSON-like string representation, allowing for easy reading and debugging.

### Major Classes/Functions Overview

- **QuantumValue Class**:
  - **Purpose**: Represents a value in the Quantum Language, utilizing `std::variant` to store different data types.
  - **Methods**:
    - `isTruthy()`: Determines if the value is considered "truthy" based on its type.
    - `toString()`: Converts the value to a string representation, tailored for each data type.

- **Utility Functions**:
  - These include helper functions used internally by the `QuantumValue` class, such as those for checking the type of a variant and performing specific conversions.

### Tradeoffs

1. **Type Safety vs. Complexity**: Using `std::variant` provides strong type safety but increases complexity compared to traditional union-based approaches. The visitor pattern further adds overhead by requiring explicit visitation logic for each type.

2. **Performance**: While the optimization for integer conversion improves performance, the use of `std::ostringstream` for double conversion might introduce some overhead due to dynamic memory allocation and formatting.

3. **Flexibility vs. Code Duplication**: The visitor pattern promotes flexibility by allowing each type to define its own behavior, but it also leads to code duplication across different visitors.

Overall, `Value.cpp` is a well-designed module that balances type safety, flexibility, and performance, making it an essential part of the Quantum Language compiler's architecture.