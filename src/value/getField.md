# `getField` Function

## Purpose
The `getField` function retrieves a field or method from an instance of a class in the Quantum Language compiler. This allows for dynamic access to properties and behaviors encapsulated within objects at runtime, facilitating flexible and powerful object-oriented programming capabilities.

## Parameters
- **name** (`std::string`): The name of the field or method to be retrieved.

## Return Value
- Returns a `QuantumValue` containing the field or method if found. If not found, throws a `NameError`.

## How It Works
1. **Field Lookup**: The function first attempts to find the specified `name` in the `fields` map associated with the instance. If found, it returns the corresponding `QuantumValue`.
2. **Method Lookup**: If the field is not found, the function proceeds to check the methods of the instance's class. It iterates through the class hierarchy using a pointer to the base class (`k`). For each class, it looks up the `name` in the `methods` map. If a matching method is found, it returns a `QuantumValue` wrapping that method.
3. **Exception Handling**: If neither a field nor a method named `name` is found in the instance or any of its ancestor classes, the function throws a `NameError`, indicating that the requested field or method does not exist on the given instance.

## Edge Cases
- **Non-existent Field/Method**: If the specified `name` does not correspond to any field or method in the instance or its class hierarchy, a `NameError` is thrown.
- **Private Fields/Methods**: The current implementation does not handle private fields or methods. Accessing such members would result in a `NameError`.
- **Overloaded Methods**: The function currently does not support overloaded methods. Attempting to retrieve a method that has multiple overloads will result in a `NameError`.

## Interactions With Other Components
- **Class Hierarchy Management**: `getField` interacts with the class hierarchy management system to traverse up the inheritance chain when searching for methods.
- **Runtime Environment**: It operates within the Quantum Language runtime environment, where instances of classes are created and managed dynamically.
- **Error Handling Mechanism**: The function utilizes the Quantum Language error handling mechanism to manage exceptions related to non-existent fields or methods.

This comprehensive approach ensures that `getField` can effectively retrieve both fields and methods from instances, providing robust support for dynamic object manipulation in the Quantum Language compiler.