# `callBuiltinMethod`

## Purpose

The `callBuiltinMethod` function is designed to handle the invocation of built-in methods on various types of objects within the Quantum Language compiler's virtual machine (VM). This function ensures that the correct method implementation is found and executed based on the object type and method name provided.

## Functionality

### Object Type Checks

1. **Array**: If the object (`obj`) is an array, the function calls `callArrayMethod`, passing the array, method name, and arguments.
2. **String**: If the object is a string, the function calls `callStringMethod`, providing the string, method name, and arguments.
3. **Dictionary**: For dictionary objects, `callDictMethod` is invoked, along with the dictionary, method name, and arguments.
4. **Instance**: If the object is an instance of a class, the function proceeds to search for the method within the class hierarchy.

### Class Hierarchy Search

- The function retrieves the class (`klass`) associated with the instance (`inst`).
- It then iterates through the class hierarchy using a `while` loop, starting from the current class and moving up to its base classes.
- During each iteration, it checks if the method exists in the current class using the `find` method on the `methods` map.
- If the method is found, the function casts the method pointer to `QuantumNative*` and looks up the corresponding closure in the `s_closureTable`.
- If the closure is found, the function pushes the object (`self`) and its arguments onto the stack, then calls the closure using `callClosure`.

### Field Access

If the method is not found in the class hierarchy, the function attempts to access the field with the given method name using `inst->getField(method)`. If successful, the field value is returned; otherwise, an exception is thrown.

### Error Handling

- If none of the above conditions are met, indicating that the method or field does not exist, the function throws a `TypeError` with a message specifying the missing method or field and the line number where the error occurred.

## Parameters/Return Value

- **Parameters**:
  - `obj`: The object on which the method is being called.
  - `method`: A string representing the name of the method to be invoked.
  - `args`: A vector of arguments passed to the method.
  
- **Return Value**:
  - Returns the result of the method call as an `Object`.

## Edge Cases

- **Non-existent Method**: If the specified method does not exist on the object, the function will throw a `TypeError`.
- **Empty Arguments Vector**: The function can handle an empty `args` vector gracefully without errors.
- **Null or Invalid Objects**: The function assumes that `obj` is a valid object. Passing null or invalid objects could lead to undefined behavior.

## Interactions with Other Components

- **Virtual Machine Stack**: The function interacts with the VM stack to push and pop objects during the method call process.
- **Class Hierarchy**: It uses the class hierarchy to find the appropriate method implementation.
- **Closure Table**: The function accesses the `s_closureTable` to retrieve closures associated with method pointers.
- **Error Handling Mechanism**: It relies on the `TypeError` exception mechanism to report errors when a method or field is not found.

This comprehensive approach ensures that the `callBuiltinMethod` function is robust and capable of handling different object types and method invocations efficiently within the Quantum Language compiler's VM.