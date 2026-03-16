# `getField` Function

## Purpose
The `getField` function is designed to retrieve a field or method from an instance of a class in the Quantum Language compiler. It first checks if the specified field or method exists directly on the instance's class. If found, it returns its value. If not, it proceeds to search up the inheritance chain until the field or method is found or the end of the chain is reached.

## Parameters
- `name`: A string representing the name of the field or method to be retrieved.

## Return Value
- Returns the value of the specified field or method as a `QuantumValue`.
- Throws a `NameError` exception if the field or method does not exist anywhere in the inheritance chain.

## Edge Cases
- **Field Not Found**: If the specified field is not present in the instance's class or any of its parent classes, the function throws a `NameError`. This ensures that only valid fields and methods can be accessed.
- **Method Overriding**: If multiple classes in the inheritance chain contain a method with the same name, the method defined in the most derived class will be returned. This aligns with the principles of method overriding in object-oriented programming.
- **Empty Inheritance Chain**: If the instance's class has no base class (i.e., it is at the top of the inheritance hierarchy), the function will still check if the field or method exists directly on the instance's class before throwing an error.

## Interactions with Other Components
- **Class Definition**: The function relies on the `klass` member variable of the instance, which holds a pointer to the class definition of the instance.
- **Fields Map**: Each class definition contains a map (`fields`) that stores the names and values of all fields defined within that class.
- **Methods Map**: Similarly, each class definition contains another map (`methods`) that stores the names and pointers to all methods defined within that class.
- **Inheritance Base**: The function uses the `base` member variable of the class definition to traverse up the inheritance chain, checking each parent class for the presence of the specified field or method.

This comprehensive approach ensures that `getField` can effectively locate and return fields and methods from instances across various levels of the inheritance hierarchy, providing robust support for object-oriented operations in the Quantum Language compiler.