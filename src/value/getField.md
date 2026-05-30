# `getField` Function

## Purpose
The `getField` function is designed to retrieve a field or method from an instance of a class in the Quantum Language compiler. It allows dynamic access to properties and behaviors encapsulated within objects at runtime, thereby enabling flexible and powerful object-oriented programming capabilities.

## Parameters
- `name`: A string representing the name of the field or method to be retrieved.

## Return Value
- Returns a `QuantumValue` containing the field or method if found.
- Throws a `NameError` exception if the specified field or method does not exist on the instance of the class.

## How It Works
1. **Field Search**: The function first searches for the specified `name` within the `fields` map associated with the current instance. If the field is found, it is returned immediately.
2. **Method Search**: If the field is not found, the function proceeds to search for the `name` as a method within the class hierarchy. It starts from the current class (`klass`) and iterates up through its base classes using a `while` loop.
   - For each class, it checks the `methods` map to see if the `name` corresponds to a method.
   - If a method is found, it is wrapped in a `QuantumValue` and returned.
3. **Hierarchy Traversal**: The traversal continues upwards until either the method is found or the base class pointer becomes `nullptr`, indicating that the end of the class hierarchy has been reached without finding the specified field or method.
4. **Exception Handling**: If the function exhausts all possibilities without finding the specified field or method, it throws a `NameError` exception, providing a clear error message indicating the absence of the field or method on the given instance.

## Edge Cases
- **Non-existent Field/Method**: If the specified field or method does not exist anywhere in the class hierarchy, the function will throw a `NameError`.
- **Base Class Pointer Becomes `nullptr`**: The function ensures that the traversal stops when the base class pointer reaches `nullptr`, preventing potential segmentation faults or undefined behavior.

## Interactions with Other Components
- **Class Hierarchy Management**: The function interacts with the class hierarchy management system to traverse up through base classes.
- **Field and Method Storage**: It accesses the `fields` and `methods` maps stored within the class instances and their respective base classes.
- **Dynamic Access**: This function facilitates dynamic access to fields and methods, which is crucial for implementing features like reflection and introspection in the Quantum Language compiler.

By combining these mechanisms, the `getField` function provides a robust solution for retrieving fields and methods dynamically, enhancing the flexibility and power of the Quantum Language compiler's object model.