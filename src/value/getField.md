# `getField` Function

## Purpose
The `getField` function is designed to retrieve a field or method from an instance of a class in the Quantum Language compiler. It plays a crucial role in allowing access to properties and behaviors encapsulated within objects at runtime.

## Parameters
- **name**: A string representing the name of the field or method to be retrieved. This parameter specifies which member of the class should be accessed.

## Return Value
- Returns a `QuantumValue` object containing either the requested field or method. If the field or method exists, it is returned directly; otherwise, it is wrapped in a `QuantumValue`.

## How It Works
The function operates in two primary steps:
1. **Field Search**: It first searches for the specified field (`name`) within the current class's `fields` map. If the field is found, it is immediately returned as a `QuantumValue`.
2. **Method Search**: If the field is not found, the function proceeds to search for the method in the class hierarchy. It starts with the current class (`klass`) and iterates through its base classes using a pointer (`k`). For each base class, it checks the `methods` map. If the method is found, it is returned wrapped in a `QuantumValue`. The search continues up the inheritance chain until the root class is reached without finding the method.

If neither the field nor any method matching the provided name is found in the class or its hierarchy, the function throws a `NameError`, indicating that the specified field or method does not exist on the given class instance.

## Edge Cases
- **Non-existent Field/Method**: If the specified field or method does not exist anywhere in the class or its base classes, the function will throw a `NameError`.
- **Ambiguity**: In a scenario where multiple base classes contain a field or method with the same name, the function will return the one from the closest ancestor class in the inheritance chain. This ensures that the correct member is accessed based on the most derived class.

## Interactions with Other Components
The `getField` function interacts closely with several key components of the Quantum Language compiler:
- **Class Definition**: It relies on the `klass` parameter to identify the class whose members are being searched.
- **Fields Map**: Each class has a `fields` map that stores all the fields defined within it. The function uses this map to quickly locate the specified field.
- **Methods Map**: Similarly, each class has a `methods` map that contains all the methods available. The function checks these maps in the base classes to ensure that inherited methods can also be accessed.
- **Inheritance Hierarchy**: The function traverses the inheritance hierarchy using the `base` pointer of each class. This allows it to search for members in parent classes when they are not present in the current class.

Overall, the `getField` function is a fundamental utility in the Quantum Language compiler, enabling dynamic access to class members across different levels of inheritance. Its implementation ensures efficient searching and retrieval of fields and methods, providing robust support for object-oriented programming features in the language.