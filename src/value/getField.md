# `getField` Function

## Purpose
The `getField` function retrieves a field or method from an instance of a class in the Quantum Language compiler. This function is crucial for accessing properties and behaviors encapsulated within objects during runtime.

## Parameters
- `name`: A string representing the name of the field or method to be retrieved.

## Return Value
- Returns a `QuantumValue` object containing the value of the field or method if found.
- Throws a `NameError` exception if the specified field or method does not exist on the instance's class or any of its base classes.

## How It Works
1. **Direct Field Lookup**: The function starts by searching for the specified `name` in the `fields` map associated with the instance's class. If the field is found, its value is returned immediately.
2. **Method Lookup in Base Classes**: If the field is not found directly, the function proceeds to search for the `name` in the methods map of each base class of the instance's class. This is done using a while loop that continues until there are no more base classes to check (`k->base.get()` returns `nullptr`).
3. **Exception Handling**: If the field or method is not found after checking all direct and inherited members, a `NameError` exception is thrown, indicating that the specified field or method does not exist on the instance.

## Edge Cases
- **Non-existent Field/Method**: If the specified field or method does not exist anywhere in the class hierarchy, a `NameError` will be thrown.
- **Field/Method Overriding**: If the same name exists as both a field and a method in the class hierarchy, the method will be returned because methods are checked after fields.
- **Empty Class Hierarchy**: If the instance's class has no base classes, only the direct fields map is searched.

## Interactions With Other Components
- **Class Definition**: The `getField` function relies on the `klass` parameter, which represents the class definition of the instance. This class definition contains information about the fields and methods available to instances of the class.
- **Fields Map**: Each class has a `fields` map that stores the names and values of its fields. When looking up a field, the `getField` function searches this map.
- **Methods Map**: Similarly, each class has a `methods` map that stores the names and implementations of its methods. When looking up a method, the `getField` function searches this map.
- **Base Classes**: The `getField` function also interacts with the `base` pointer of the class definition, which points to the parent class. This allows the function to traverse up the class hierarchy when searching for fields and methods.

By following this structured approach, the `getField` function ensures that fields and methods can be accessed correctly even when they are defined in ancestor classes, providing a robust mechanism for object-oriented programming in the Quantum Language compiler.