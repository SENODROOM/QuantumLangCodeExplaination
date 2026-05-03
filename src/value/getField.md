# `getField` Function

## Purpose
The `getField` function retrieves a field or method from an instance of a class in the Quantum Language compiler. This function is essential for accessing properties and behaviors encapsulated within objects during runtime.

## Parameters
- `name`: A string representing the name of the field or method to retrieve. This parameter specifies which property or behavior should be accessed from the object.

## Return Value
The function returns a `QuantumValue` object that represents either the field or method associated with the given name. If the specified field or method exists, it is returned successfully.

## Edge Cases
1. **Non-existent Field/Method**: If the provided `name` does not correspond to any existing field or method in the object's class hierarchy, the function throws a `NameError`. This ensures that the user is aware of attempting to access something that doesn't exist.
2. **Case Sensitivity**: The function is case-sensitive when searching for fields and methods. It will only find matches that exactly match the case of the provided `name`.

## How It Works
The `getField` function operates in two main steps:
1. **Field Search**: First, it checks if the field named `name` exists directly within the object's current class (`klass`). If found, it returns the corresponding `QuantumValue`.
2. **Method Search**: If the field is not found in the current class, the function proceeds to search up the class hierarchy. It starts by retrieving the base class of the current class (`k = k->base.get()`). Then, it iterates through all classes in the hierarchy, checking each one for a method named `name`. If a method is found, it is wrapped in a `QuantumValue` and returned. If no field or method is found after traversing the entire hierarchy, a `NameError` is thrown.

This approach allows `getField` to dynamically access both fields and methods at runtime, providing flexibility and power to the Quantum Language compiler.

## Interactions with Other Components
The `getField` function interacts with several key components of the Quantum Language compiler:
- **Class Hierarchy**: It uses the class hierarchy to traverse upwards from the object's class to its base classes, searching for the requested field or method.
- **Fields Storage**: Fields are stored in a map called `fields`, making quick lookup possible.
- **Methods Storage**: Methods are also stored in a map called `methods`, allowing similar fast retrieval as for fields.
- **Exception Handling**: When a non-existent field or method is accessed, the function throws a `NameError`, which is handled by the exception system of the compiler.

By leveraging these components, `getField` provides robust functionality for accessing object properties and behaviors, enhancing the capabilities of the Quantum Language compiler.