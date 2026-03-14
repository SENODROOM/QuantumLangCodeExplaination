# evalNewExpr() Function Explanation

## Complete Code

```cpp
QuantumValue Interpreter::evalNewExpr(NewExpr &e)
{
    static const std::unordered_set<std::string> primitives = {
        "int", "long", "short", "unsigned", "float", "double", "char", "bool"};
    
    std::string typeName = e.type;
    if (primitives.count(typeName))
    {
        // Create primitive value with default initialization
        if (typeName == "int" || typeName == "long" || typeName == "short" || typeName == "unsigned")
            return QuantumValue(0LL);
        if (typeName == "float" || typeName == "double")
            return QuantumValue(0.0);
        if (typeName == "char")
            return QuantumValue(std::string(1, '\0'));
        if (typeName == "bool")
            return QuantumValue(false);
    }
    
    // Treat as class instantiation
    auto klassVal = evaluate(*e.typeExpr);
    if (!klassVal.isClass())
        throw TypeError("Cannot instantiate non-class: " + klassVal.typeName());
    
    auto klass = klassVal.asClass();
    auto instance = std::make_shared<QuantumInstance>(klass);
    
    // Call constructor if exists
    auto ctor = klass->findMethod("<init>");
    if (ctor)
    {
        std::vector<QuantumValue> args;
        args.reserve(e.arguments.size());
        for (auto &arg : e.arguments)
            args.push_back(evaluate(*arg));
        callInstanceMethod(instance, ctor, std::move(args));
    }
    
    return QuantumValue(instance);
}
```

## Line-by-Line Explanation

### Function Signature (Line 4261)
- **Line 4261**: `QuantumValue Interpreter::evalNewExpr(NewExpr &e)` - Evaluate new expressions
  - `e`: Reference to NewExpr AST node
  - Returns QuantumValue containing the created object

### Primitive Types Registry (Lines 4262-4265)
- **Line 4262**: `{` - Opening brace
- **Line 4263**: `static const std::unordered_set<std::string> primitives = {` - Define primitive types set
- **Line 4264**: `"int", "long", "short", "unsigned", "float", "double", "char", "bool"};` - C++ primitive types
- **Line 4265**: Empty line for readability

### Type Name Extraction (Line 4266)
- **Line 4266**: `std::string typeName = e.type;` - Get type name from expression

### Primitive Type Handling (Lines 4267-4280)
- **Line 4267**: `if (primitives.count(typeName))` - Check if type is primitive
- **Line 4268**: `{` - Opening brace for primitive case
- **Line 4269**: `// Create primitive value with default initialization` - Comment about primitive creation
- **Line 4270**: `if (typeName == "int" || typeName == "long" || typeName == "short" || typeName == "unsigned")` - Integer types
- **Line 4271**: `return QuantumValue(0LL);` - Return zero integer
- **Line 4272**: `if (typeName == "float" || typeName == "double")` - Float types
- **Line 4273**: `return QuantumValue(0.0);` - Return zero float
- **Line 4274**: `if (typeName == "char")` - Character type
- **Line 4275**: `return QuantumValue(std::string(1, '\0'));` - Return null character
- **Line 4276**: `if (typeName == "bool")` - Boolean type
- **Line 4277**: `return QuantumValue(false);` - Return false
- **Line 4278**: `}` - Closing brace for primitive case

### Class Instantiation (Lines 4279-4282)
- **Line 4279**: Empty line for readability
- **Line 4280**: `// Treat as class instantiation` - Comment about class creation
- **Line 4281**: `auto klassVal = evaluate(*e.typeExpr);` - Evaluate class expression
- **Line 4282**: `if (!klassVal.isClass())` - Check if result is class

### Type Validation (Lines 4283-4285)
- **Line 4283**: `throw TypeError("Cannot instantiate non-class: " + klassVal.typeName());` - Error for non-class
- **Line 4284**: Empty line for readability
- **Line 4285**: `auto klass = klassVal.asClass();` - Get class object

### Instance Creation (Lines 4286-4290)
- **Line 4286**: `auto instance = std::make_shared<QuantumInstance>(klass);` - Create instance object
- **Line 4287**: Empty line for readability
- **Line 4288**: `// Call constructor if exists` - Comment about constructor call
- **Line 4289**: `auto ctor = klass->findMethod("<init>");` - Find constructor method
- **Line 4290**: `if (ctor)` - Check if constructor exists

### Constructor Call (Lines 4291-4299)
- **Line 4291**: `{` - Opening brace for constructor case
- **Line 4292**: `std::vector<QuantumValue> args;` - Create arguments vector
- **Line 4293**: `args.reserve(e.arguments.size());` - Reserve space for arguments
- **Line 4294**: `for (auto &arg : e.arguments)` - Loop through arguments
- **Line 4295**: `args.push_back(evaluate(*arg));` - Evaluate and add argument
- **Line 4296**: `callInstanceMethod(instance, ctor, std::move(args));` - Call constructor
- **Line 4297**: `}` - Closing brace for constructor case
- **Line 4298**: Empty line for readability
- **Line 4299**: `return QuantumValue(instance);` - Return instance value
- **Line 4300**: `}` - Closing brace for function

## Summary

The `evalNewExpr()` function handles object creation with both primitive types and class instantiation in the Quantum Language:

### Key Features
- **Primitive Types**: Creates default-initialized primitive values
- **Class Instantiation**: Creates object instances with constructor calls
- **Constructor Support**: Automatically calls class constructors if they exist
- **Type Safety**: Validates type expressions before instantiation

### New Expression Types
- **Primitive Creation**: `new int` - creates primitive values
- **Class Instantiation**: `new ClassName(args)` - creates object instances
- **Constructor Calls**: `new Person("John", 25)` - calls constructors
- **Default Initialization**: Primitives get default values

### Primitive Type Handling
- **Integer Types**: int, long, short, unsigned → 0
- **Float Types**: float, double → 0.0
- **Character Type**: char → '\0' (null character)
- **Boolean Type**: bool → false

### Class Instantiation Process
1. **Type Evaluation**: Evaluate the class expression
2. **Type Validation**: Ensure result is a class type
3. **Instance Creation**: Create QuantumInstance object
4. **Constructor Lookup**: Find constructor method
5. **Argument Evaluation**: Evaluate constructor arguments
6. **Constructor Call**: Call constructor with arguments
7. **Instance Return**: Return created instance

### Constructor Support
- **Constructor Name**: `<init>` method name for constructors
- **Argument Passing**: Supports constructor arguments
- **Optional Constructor**: No error if constructor doesn't exist
- **Instance Binding**: Constructor called with proper instance binding

### Design Benefits
- **C Compatibility**: Familiar C++ new operator syntax
- **Type Safety**: Comprehensive type checking
- **Object-Oriented**: Proper constructor semantics
- **Memory Management**: Smart pointers for instance lifecycle

### Use Cases
- **Primitive Creation**: `new int` for primitive values
- **Object Creation**: `new Person()` for object instances
- **Constructor Arguments**: `new Person("John", 25)` with parameters
- **Complex Objects**: `new ComplexObject(arg1, arg2)`

### Integration with Other Components
- **Class System**: Uses QuantumClass for type information
- **Instance System**: Uses QuantumInstance for objects
- **Constructor System**: Uses `<init>` method for constructors
- **Method System**: Uses callInstanceMethod for constructor calls

### Performance Characteristics
- **Primitive Creation**: O(1) primitive value creation
- **Instance Creation**: O(1) instance object creation
- **Constructor Call**: O(n) where n is constructor complexity
- **Memory Management**: Automatic cleanup through shared pointers

### New Expression Examples
- **Primitive**: `new int` → `0`
- **Float**: `new double` → `0.0`
- **Character**: `new char` → `'\0'`
- **Boolean**: `new bool` → `false`
- **Class**: `new Person()` → Person instance
- **With Args**: `new Person("John", 25)` → constructed instance

### Error Handling
- **Type Errors**: TypeError for non-class instantiation
- **Constructor Errors**: Errors in constructor evaluation propagate
- **Argument Errors**: Errors in argument evaluation propagate
- **Memory Errors**: Handled by smart pointer system

### Memory Management
- **Shared Pointers**: Instances use std::shared_ptr
- **Automatic Cleanup**: Memory freed when no references remain
- **Reference Counting**: Tracks instance usage
- **Constructor Safety**: Proper cleanup if constructor fails

### Constructor Semantics
- **Method Name**: `<init>` for constructor identification
- **Instance Binding**: Constructor called with instance as `this`
- **Argument Passing**: Standard argument passing semantics
- **Return Value**: Constructor returns the instance

This function provides the foundation for object creation in the Quantum Language, enabling both primitive value creation and class instantiation with proper constructor support while maintaining type safety and memory management throughout the object creation process.
