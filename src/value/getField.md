# `getField` Function

## Purpose
The `getField` function is designed to retrieve a field or method from an instance of a class in the Quantum Language compiler. It allows dynamic access to properties and behaviors encapsulated within objects at runtime, thereby enabling flexible and powerful object-oriented programming capabilities.

## Parameters
- **name**: A string representing the name of the field or method to be retrieved.

## Return Value
- Returns a `QuantumValue` containing the field or method if found.
- Throws a `NameError` if no field or method with the specified name exists on the instance of the class.

## Edge Cases
1. **Non-existent Field/Method**: If the specified field or method does not exist on the instance of the class, the function will throw a `NameError`.
2. **Inheritance Chain**: The function searches for the field or method in the current class and then recursively checks each base class in the inheritance chain until it finds the desired member or reaches the topmost base class.
3. **Private Members**: The function should handle private members appropriately, ensuring that they can only be accessed through public interfaces or when explicitly allowed.

## Interactions with Other Components
- **Class Definition**: The function interacts with the class definition (`klass`) to access its fields and methods.
- **Field Storage**: Fields are stored in a map called `fields`, which allows for efficient lookup.
- **Method Storage**: Methods are stored in another map called `methods`, similar to fields, allowing for quick retrieval.
- **Base Class Handling**: When a field or method is not found in the current class, the function traverses up the inheritance chain using the `base` pointer of the class definition.

## Implementation Details
The implementation of the `getField` function involves two main steps:
1. **Local Lookup**: The function first attempts to find the specified field or method in the local `fields` map of the instance's class.
2. **Inheritance Search**: If the local lookup fails, the function proceeds to search the inheritance chain. This is done by iterating through each base class of the current class until the desired member is found or the topmost base class is reached.

Here is the code snippet for the `getField` function:

```cpp
auto it = fields.find(name);
if (it != fields.end())
    return it->second;

// Check methods
auto k = klass.get();
while (k)
{
    auto mit = k->methods.find(name);
    if (mit != k->methods.end())
        return QuantumValue(mit->second);
    k = k->base.get();
}

throw NameError("No field/method '" + name + "' on instance of " + klass->name);
```

### Explanation of Code
- **Line 1**: The function attempts to find the specified field or method in the `fields` map of the instance's class.
- **Line 2-4**: If the field is found, it is returned directly.
- **Line 5**: The function then starts checking methods by retrieving the current class definition (`klass`).
- **Line 6-8**: It iterates through each base class of the current class, searching for the specified method in the `methods` map of each base class.
- **Line 9**: If the method is found, it is wrapped in a `QuantumValue` and returned.
- **Line 10-12**: If neither the field nor the method is found after checking all classes in the inheritance chain, a `NameError` is thrown indicating that the specified member does not exist on the instance of the class.

This design ensures that the `getField` function can dynamically access both fields and methods, providing flexibility and power in handling objects at runtime.