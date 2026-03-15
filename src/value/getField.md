# `getField` Function

## Purpose
The `getField` function is designed to retrieve a field or method from an instance of a class in the Quantum Language compiler. If the specified field or method exists directly on the instance's class, it returns its value. If not, it searches up the inheritance chain until it finds the field or method or reaches the end of the chain without finding it.

## Parameters
- **name**: A string representing the name of the field or method to be retrieved.

## Return Value
- **QuantumValue**: Returns the value of the field or method if found.
- **NameError**: Throws an exception if the field or method is not found on the instance or any of its ancestor classes.

## Edge Cases
1. **Field Not Found**: If the specified field does not exist on the instance's class or any of its ancestor classes, a `NameError` will be thrown.
2. **Method Not Found**: Similar to the field case, if the specified method does not exist, a `NameError` will be thrown.
3. **Empty Class Name**: The function assumes that the class name is non-empty and will throw a `NameError` if it encounters an empty class name during the search.

## Interactions with Other Components
- **Class Hierarchy Management**: This function interacts with the class hierarchy management system to traverse up the inheritance chain.
- **Field Storage**: It accesses the `fields` map within the instance's class to check for the existence of the specified field.
- **Method Storage**: It also checks the `methods` map within each class in the inheritance chain to find the specified method.
- **Exception Handling**: The function uses exception handling (`throw`) to manage errors when the specified field or method is not found.

## Detailed Explanation
### Step-by-Step Execution
1. **Search Directly on Instance**:
   - The function first attempts to find the specified `name` in the `fields` map of the instance's class.
   - If the field is found (`it != fields.end()`), it returns the corresponding `QuantumValue`.

2. **Traverse Inheritance Chain**:
   - If the field is not found, the function retrieves the base class of the current class using `klass.get()`.
   - It then enters a loop where it continues to search the `methods` map of the base class.
   - If the method is found (`mit != k->methods.end()`), it constructs a `QuantumValue` from the method and returns it.
   - The function then moves to the next base class by setting `k = k->base.get()` and repeats the process until it either finds the field or method or reaches the top of the inheritance chain.

3. **Throw Exception**:
   - If the function exhausts all possible classes in the inheritance chain without finding the specified field or method, it throws a `NameError` indicating that the field or method is not present on the instance.

### Example Usage
```cpp
// Assuming 'instance' is an object of a class named 'MyClass'
auto fieldValue = instance.getField("myField");
auto methodValue = instance.getField("myMethod");
```

In this example, `getField` is called twice on an instance of `MyClass`. Once to retrieve a field named `myField`, and once to retrieve a method named `myMethod`. If both are found, their values are returned; otherwise, a `NameError` would be thrown.

### Error Handling
To handle potential errors gracefully, you can catch the `NameError` as follows:

```cpp
try {
    auto fieldValue = instance.getField("nonExistentField");
} catch (const NameError& e) {
    std::cerr << e.what() << std::endl;  // Output: No field/method 'nonExistentField' on instance of MyClass
}
```

This ensures that your program can continue running even if a field or method is missing from the class hierarchy.