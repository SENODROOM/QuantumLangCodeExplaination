# Value.cpp - Core Data Representation Module of Quantum Language Compiler

## Overview

`Value.cpp` is a crucial component of the Quantum Language compiler, responsible for handling and manipulating various types of values within the language's runtime environment. This module defines the `QuantumValue` class and related functions to manage these values efficiently and accurately.

## Design Decisions

### Use of `std::variant` for Flexible Types

The primary design decision in `Value.cpp` is the use of `std::variant` to represent different types of values (`QuantumNil`, `bool`, `double`, `std::string`, `Array`, `QuantumPointer`, `QuantumFunction`, `QuantumNative`). This choice provides flexibility and type safety without manual type checking and casting, enhancing both performance and maintainability.

### Overloaded Functions for Type-Specific Operations

To handle operations specific to each value type, overloaded functions (`isTruthy`, `toString`) utilize `std::visit` to dispatch based on the actual type stored in `QuantumValue`. This approach ensures that each operation is performed correctly and efficiently, tailored to the nature of the value.

## Classes and Functions Documentation

### QuantumValue Class

**Purpose**: Represents a value in the Quantum Language runtime environment. It encapsulates multiple possible types and provides methods to interact with them safely and flexibly.

**Behavior**:
- **Constructor**: Initializes a `QuantumValue` with a default type.
- **Destructor**: Cleans up any resources associated with the value.
- **Assignment Operator**: Allows assignment between `QuantumValue` instances.
- **Equality Operator**: Checks if two `QuantumValue` instances are equal.
- **isTruthy Method**: Determines if the value is considered "truthy" in the context of conditional statements.
- **toString Method**: Converts the value to a string representation suitable for display or logging.

### isTruthy Function

**Purpose**: Determines whether a given `QuantumValue` is considered "truthy."

**Behavior**:
- Utilizes `std::visit` to inspect the underlying type of the value.
- Returns `false` for `QuantumNil`.
- Returns the boolean value directly for `bool` types.
- For numeric types (`double`), it checks if the value is not zero.
- For strings, it returns `true` if the string is non-empty and not just a null character.
- For arrays, it checks if the array is not empty.
- For pointers, it verifies if the pointer is valid and not pointing to a null instance.
- For other types, it defaults to returning `true`.

### toString Function

**Purpose**: Converts a `QuantumValue` to its string representation.

**Behavior**:
- Utilizes `std::visit` to convert the underlying type to a string.
- Handles `QuantumNil` by returning `"nil"`.
- For booleans, it returns `"true"` or `"false"`.
- For doubles, it formats the number to a precision of 10 digits. If the number is an integer below 1e15, it converts it to a long long string.
- For strings, it returns the string as is.
- For arrays, it constructs a string representation by iterating over the elements and converting each one to a string, enclosing them in square brackets.
- For dictionaries, it constructs a string representation by iterating over the key-value pairs and converting each pair to a string, enclosing them in curly braces.
- For function and native objects, it returns a string indicating the object type and name.

## Tradeoffs and Limitations

- **Performance**: While `std::variant` and `std::visit` provide flexibility, they can introduce some overhead compared to traditional union-based approaches. However, this is generally outweighed by the benefits of type safety and ease of use.
- **Complexity**: The introduction of multiple types and their respective conversions adds complexity to the codebase. Careful management of these types is essential to avoid bugs and ensure correct behavior.
- **Limitation on Custom Types**: Currently, only a few basic types are supported. Extending support for custom user-defined types would require additional work and could potentially complicate the existing system.

This README provides a comprehensive overview of the `Value.cpp` module, including its role in the compiler pipeline, key design decisions, and documentation of its major classes and functions.