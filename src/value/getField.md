# `getField` Function

## Purpose
The `getField` function is designed to retrieve a field or method from an instance of a class in the Quantum Language compiler. It enables dynamic access to properties and behaviors encapsulated within objects at runtime, thereby facilitating flexible and powerful object-oriented programming practices.

## Parameters
- `name`: A string representing the name of the field or method to be retrieved.

## Return Value
- Returns a `QuantumValue` containing the field or method if found.
- Throws a `NameError` exception if the specified field or method does not exist on the instance of the class.

## How it Works
1. **Field Search**: The function first attempts to find the specified `name` in the `fields` map associated with the current instance. If the field is found (`it != fields.end()`), it returns the corresponding `QuantumValue`.

2. **Method Search**: If the field is not found, the function proceeds to check for the existence of the `name` as a method in the class hierarchy. It starts by retrieving the class pointer (`klass.get()`) and then iterates through each base class using a `while` loop until it reaches the root class (`nullptr`). During each iteration, it checks the `methods` map of the current class (`k->methods.find(name)`). If a method with the specified `name` is found, it returns a new `QuantumValue` wrapping the method.

3. **Exception Handling**: If neither a field nor a method named `name` is found after searching all classes in the hierarchy, the function throws a `NameError` exception indicating that the specified field or method does not exist on the instance of the class.

## Edge Cases
- **Non-existent Field/Method**: If the specified `name` does not correspond to any field or method in the class hierarchy, the function will throw a `NameError`. This ensures that the user is aware of the incorrect field or method name they provided.
- **Inheritance Chain**: The function correctly handles inheritance chains by checking each base class for the specified `name`. This allows for accessing overridden fields and methods in derived classes.

## Interactions with Other Components
- **Class Hierarchy Management**: The `getField` function interacts with the class hierarchy management component to traverse up the inheritance chain when searching for methods.
- **Field/Method Storage**: It relies on the storage mechanisms for fields and methods, which are typically implemented as maps within the class definition.
- **Dynamic Access**: By allowing dynamic access to fields and methods, the `getField` function facilitates advanced features such as reflection and introspection, enhancing the flexibility and power of the Quantum Language compiler.

This comprehensive approach ensures that the `getField` function can effectively retrieve fields and methods from instances of classes, even in complex inheritance hierarchies, providing robust support for dynamic object-oriented programming.