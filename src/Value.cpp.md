# Value.cpp - Core Data Representation Module of Quantum Language Compiler

## Overview

`Value.cpp` is a fundamental part of the Quantum Language compiler, playing a crucial role in managing and manipulating various data types throughout the compilation process. This module encapsulates these data types within the `QuantumValue` class and provides utility functions to handle them effectively. The primary responsibilities of `Value.cpp` include:

- **Data Encapsulation**: The `QuantumValue` class represents different data types such as nil, boolean, numbers, strings, arrays, dictionaries, closures, and native functions.
- **Truthiness Check**: The `isTruthy()` function determines whether a `QuantumValue` instance is considered "truthy" or not, which is essential for control flow statements like conditionals.
- **String Conversion**: The `toString()` method converts a `QuantumValue` instance into its string representation, facilitating debugging and output operations.

## Role in Compiler Pipeline

The `Value.cpp` module operates at multiple stages of the compiler pipeline:

- **Parsing Phase**: It helps in parsing expressions and literals into their corresponding `QuantumValue` instances.
- **Semantic Analysis Phase**: During semantic analysis, it ensures that values are used correctly according to the language's rules.
- **Code Generation Phase**: It generates machine code or intermediate representations based on the `QuantumValue` instances.

## Key Design Decisions and Why

### Use of `std::variant`
The `QuantumValue` class uses `std::variant` to store different data types. This choice allows for compile-time type safety and efficient storage without the overhead of dynamic casting.

### Truthiness Implementation
The `isTruthy()` function employs template metaprogramming with `std::visit` to check the truthiness of various data types. This approach ensures that each type has a specific definition of what constitutes a "truthy" value, making the implementation both flexible and maintainable.

### String Conversion Strategy
The `toString()` method uses `std::visit` to convert different data types into their string representations. For numeric types, it handles large integers specially to avoid scientific notation. This strategy ensures that all data types can be represented accurately as strings.

## Major Classes/Functions Overview

### QuantumValue Class
- **Purpose**: Represents different data types in the Quantum Language.
- **Key Methods**:
  - `isTruthy()`: Checks if the value is considered "truthy".
  - `toString()`: Converts the value to its string representation.

### isTruthy() Function
- **Purpose**: Determines the truthiness of a `QuantumValue`.
- **Implementation**: Uses `std::visit` and template metaprogramming to handle different data types.

### toString() Method
- **Purpose**: Converts a `QuantumValue` to its string representation.
- **Implementation**: Uses `std::visit` to handle different data types, including special handling for large integers.

## Tradeoffs

### Compile-Time vs Runtime Type Safety
Using `std::variant` provides compile-time type safety but may introduce some runtime overhead compared to traditional unions or pointers to base classes.

### Flexibility vs Complexity
Template metaprogramming in `isTruthy()` offers flexibility in defining truthiness for each type but increases complexity in the implementation.

### Performance vs Accuracy
Handling large integers specially in `toString()` improves accuracy but might slightly impact performance due to additional checks and conversions.

Overall, `Value.cpp` is an integral part of the Quantum Language compiler, ensuring robust and accurate data handling across different phases of the compilation process.