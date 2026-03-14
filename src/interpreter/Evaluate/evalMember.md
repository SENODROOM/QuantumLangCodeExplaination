# evalMember() Function Explanation

## Complete Code

```cpp
QuantumValue Interpreter::evalMember(MemberExpr &e)
{
    auto obj = evaluate(*e.object);
    if (obj.isPointer())
    {
        obj = obj.asPointer()->deref();
    }
    if (obj.isInstance())
    {
        return obj.asInstance()->getField(e.member);
    }
    if (obj.isClass())
    {
        auto klass = obj.asClass();
        auto method = klass->findMethod(e.member);
        if (method)
            return QuantumValue(method);
        throw TypeError("No method '" + e.member + "' on " + obj.typeName());
    }
    throw TypeError("No member '" + e.member + "' on " + obj.typeName());
}
```

## Line-by-Line Explanation

### Function Signature (Line 3907)
- **Line 3907**: `QuantumValue Interpreter::evalMember(MemberExpr &e)` - Evaluate member access expressions
  - `e`: Reference to MemberExpr AST node
  - Returns QuantumValue result of member access

### Object Evaluation (Lines 3908-3912)
- **Line 3908**: `{` - Opening brace
- **Line 3909**: `auto obj = evaluate(*e.object);` - Evaluate object expression
- **Line 3910**: `if (obj.isPointer())` - Check if object is a pointer
- **Line 3911**: `{` - Opening brace for pointer case
- **Line 3912**: `obj = obj.asPointer()->deref();` - Dereference pointer

### Instance Field Access (Lines 3913-3917)
- **Line 3913**: `}` - Closing brace for pointer case
- **Line 3914**: `if (obj.isInstance())` - Check if object is instance
- **Line 3915**: `{` - Opening brace for instance case
- **Line 3916**: `return obj.asInstance()->getField(e.member);` - Return instance field
- **Line 3917**: `}` - Closing brace for instance case

### Class Method Access (Lines 3918-3925)
- **Line 3918**: `if (obj.isClass())` - Check if object is class
- **Line 3919**: `{` - Opening brace for class case
- **Line 3920**: `auto klass = obj.asClass();` - Get class reference
- **Line 3921**: `auto method = klass->findMethod(e.member);` - Find method in class
- **Line 3922**: `if (method)` - Check if method found
- **Line 3923**: `return QuantumValue(method);` - Return method function
- **Line 3924**: `throw TypeError("No method '" + e.member + "' on " + obj.typeName());` - Error for missing method
- **Line 3925**: `}` - Closing brace for class case

### Error Handling (Lines 3926-3929)
- **Line 3926**: Empty line for readability
- **Line 3927**: `throw TypeError("No member '" + e.member + "' on " + obj.typeName());` - Error for non-member types
- **Line 3928**: `}` - Closing brace for function

## Summary

The `evalMember()` function handles member access expressions for objects and classes in the Quantum Language:

### Key Features
- **Pointer Support**: Automatic dereferencing of pointer objects
- **Instance Fields**: Access to instance variables and properties
- **Class Methods**: Access to static methods on classes
- **Error Handling**: Clear error messages for missing members

### Member Access Types Supported
- **Instance Fields**: `object.field` - access instance variables
- **Class Methods**: `Class.method` - access static methods
- **Pointer Dereference**: `pointer.field` - auto-dereference and access

### Pointer Handling
- **Automatic Dereference**: Pointers automatically dereferenced
- **Transparent Access**: Same syntax for pointers and objects
- **Type Safety**: Proper pointer type checking
- **Memory Safety**: Safe pointer dereferencing

### Instance Field Access
- **Field Lookup**: Direct access to instance fields
- **Inheritance**: Fields from base classes accessible
- **Type Safety**: Proper field type checking
- **Error Handling**: Clear errors for missing fields

### Class Method Access
- **Method Lookup**: Search for methods in class hierarchy
- **Static Methods**: Access to class-level methods
- **Inheritance**: Methods from base classes accessible
- **Function Objects**: Methods returned as callable objects

### Member Resolution Process
1. **Object Evaluation**: Evaluate the object expression
2. **Pointer Handling**: Dereference if object is pointer
3. **Type Dispatch**: Determine object type (instance/class)
4. **Member Lookup**: Find field or method by name
5. **Value Return**: Return the found member value

### Design Benefits
- **Object-Oriented**: Proper support for OOP features
- **Pointer Integration**: Seamless pointer-to-object access
- **Inheritance Support**: Methods and fields from base classes
- **Type Safety**: Comprehensive type checking

### Use Cases
- **Instance Variables**: `obj.property` - access object properties
- **Static Methods**: `Class.method` - access class methods
- **Pointer Access**: `ptr->field` - access through pointers
- **Method Objects**: Get method objects for later calling

### Integration with Other Components
- **Instance Class**: Uses instance field storage
- **Class System**: Uses class method lookup
- **Pointer System**: Integrates with pointer dereferencing
- **Error System**: Uses TypeError for missing members

### Performance Characteristics
- **Direct Access**: O(1) field and method access
- **Pointer Overhead**: Minimal pointer dereferencing cost
- **Type Dispatch**: Efficient type checking
- **Inheritance Search**: Linear search through class hierarchy

### Member Access Examples
- **Instance Field**: `person.name` - returns person's name
- **Class Method**: `Math.sqrt` - returns sqrt function
- **Pointer Field**: `ptr->value` - auto-dereference and access
- **Missing Member**: `obj.unknown` - throws TypeError

### Error Handling
- **TypeError**: Thrown for missing members
- **Pointer Errors**: Safe pointer dereferencing
- **Type Safety**: Proper type validation
- **Descriptive Messages**: Clear error descriptions

### Inheritance Support
- **Base Class Fields**: Access fields from parent classes
- **Base Class Methods**: Access methods from parent classes
- **Method Override**: Derived class methods override base methods
- **Search Order**: Current class first, then base classes

This function provides the foundation for object-oriented programming in the Quantum Language, enabling proper member access for instances and classes while maintaining seamless pointer integration and comprehensive error handling throughout the member access process.
