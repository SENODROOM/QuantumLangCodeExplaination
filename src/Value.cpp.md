# Value.cpp - Core Data Representation Module of Quantum Language Compiler

## Overview

`Value.cpp` is a crucial part of the Quantum Language compiler, focusing on managing and manipulating different types of values within the language's runtime environment. This module introduces the `QuantumValue` class and associated functions to ensure efficient and accurate handling of these values.

## Role in the Compiler Pipeline

`Value.cpp` serves as a foundational layer in the Quantum Language compiler pipeline. It encapsulates the representation and behavior of all value types used in the language, enabling seamless integration throughout the compilation process. The primary responsibilities include:

- **Data Storage**: Storing various types of values such as integers, booleans, strings, arrays, dictionaries, functions, and native objects.
- **Type Checking**: Performing type checks to ensure operations are performed on compatible value types.
- **Evaluation**: Evaluating expressions and statements involving these values.
- **Conversion**: Converting values between different types when necessary.
- **Comparison**: Comparing values to determine truthiness or equality.

## Key Design Decisions and Justifications

### Use of `std::variant` for Type Storage

The decision to use `std::variant` for storing different types of values was driven by the need for flexibility and safety. `std::variant` allows for compile-time type checking, ensuring that operations are always performed on valid types without the risk of undefined behavior. This choice enhances both performance and reliability compared to traditional union-based approaches.

### Visitor Pattern for Operations

Implementing the visitor pattern for operations on `QuantumValue` instances ensures that each operation can be handled appropriately based on the underlying type. This approach avoids the need for multiple type-checks and explicit casting, making the code cleaner and more maintainable. By leveraging `std::visit`, we can easily extend the functionality of the `QuantumValue` class without modifying existing code.

## Documentation of Major Classes/Functions

### QuantumValue Class

**Purpose**: Represents a value in the Quantum Language runtime environment, capable of holding various types including integers, booleans, strings, arrays, dictionaries, functions, and native objects.

**Behavior**:
- **Constructor**: Initializes a `QuantumValue` instance with a default value of `QuantumNil`.
- **Destructor**: Cleans up any dynamically allocated resources held by the value.
- **Assignment Operators**: Overloaded assignment operators to copy or move values into a `QuantumValue` instance.
- **Type Accessors**: Provides methods to access the underlying type of the value, such as `isInteger()`, `isBoolean()`, etc.
- **Operations**: Defines methods like `add()`, `subtract()`, `multiply()`, etc., which utilize the visitor pattern to perform operations based on the value's type.

### isTruthy Function

**Purpose**: Determines whether a given `QuantumValue` instance is considered "truthy" in the context of conditional statements.

**Behavior**:
- Utilizes `std::visit` to apply a lambda function to the value, which checks the truthiness based on the underlying type.
- Returns `false` for `QuantumNil`, `true` for boolean values, non-zero doubles, non-empty strings, non-empty arrays, and non-null pointers.
- For other types, it returns `true`.

### toString Function

**Purpose**: Converts a `QuantumValue` instance to its string representation.

**Behavior**:
- Utilizes `std::visit` to apply a lambda function to the value, which converts the value to a string based on its underlying type.
- Handles specific cases like `QuantumNil`, booleans, doubles, strings, arrays, dictionaries, functions, and native objects.
- Ensures proper formatting for numbers and includes quotes around strings and array elements.

## Tradeoffs and Limitations

- **Performance**: While `std::variant` provides type safety, it may introduce some overhead compared to simpler data structures. However, this is mitigated by careful optimization and profiling.
- **Complexity**: The visitor pattern adds complexity to the codebase, particularly when adding new types or operations. This requires thorough testing to ensure consistency and correctness.
- **Memory Usage**: Dynamic memory allocation is used extensively for storing complex types like arrays and dictionaries. Efficient management of memory is essential to prevent leaks and optimize resource usage.

## Conclusion

`Value.cpp` is a vital component of the Quantum Language compiler, offering robust support for managing and manipulating various value types. Its design choices, such as using `std::variant` and the visitor pattern, provide both safety and flexibility, although they come with their own set of challenges. By carefully documenting and addressing these issues, the module continues to serve as a cornerstone of the compiler's functionality.