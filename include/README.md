# QuantumLanguage Compiler - Value.h

## Overview

The `include/Value.h` header file is a crucial component of the QuantumLanguage compiler, focusing on defining the data types and structures used within the virtual machine (VM). This header file plays a vital role in the compiler's pipeline by providing a robust framework for managing values during compilation and execution.

## Role in Compiler Pipeline

The `Value.h` header file is integral to several stages of the QuantumLanguage compiler:

1. **Lexical Analysis**: It helps in identifying and categorizing tokens based on their type.
2. **Syntax Analysis**: During parsing, it defines how different token types can be combined into valid expressions or statements.
3. **Semantic Analysis**: It aids in validating the semantics of the parsed code, ensuring that operations are performed correctly.
4. **Code Generation**: It provides the necessary data structures and functions to generate intermediate representations or target code.
5. **Execution**: Finally, it supports the execution phase by defining how values are manipulated and stored during runtime.

## Key Design Decisions and Why

One of the primary design decisions in `Value.h` is the use of `std::variant` to represent different value types. This choice was made because:

- **Flexibility**: `std::variant` allows for multiple possible types within a single variable, making it easier to handle various data types without resorting to unions or complex type hierarchies.
- **Safety**: Unlike raw pointers, `std::variant` ensures type safety at compile time, reducing the risk of undefined behavior due to incorrect type access.
- **Performance**: Modern compilers are highly optimized for `std::variant`, resulting in efficient memory usage and performance.

Another significant decision is the introduction of `QuantumPointer`. This structure encapsulates a shared pointer to another `QuantumValue` and includes additional metadata such as a variable name and an offset. The reasons behind this design include:

- **Memory Management**: By using a shared pointer, `QuantumPointer` simplifies memory management, automatically handling the allocation and deallocation of pointed-to objects.
- **Debugging and Display**: Including the variable name in `QuantumPointer` facilitates better debugging and display of variables, which is essential for understanding and maintaining the code.
- **Arithmetic Operations**: The offset field allows for pointer arithmetic, enabling more complex data manipulation and access patterns.

## Major Classes/Functions Overview

### QuantumValue

The `QuantumValue` class is the central data structure representing all values in the QuantumLanguage compiler. It uses `std::variant` to store different types of values, including `QuantumNil`, `bool`, `double`, strings, arrays, dictionaries, closures, native functions, instances, classes, bound methods, and pointers.

#### Example Usage:
```cpp
QuantumValue val = true;
if (val.isBool()) {
    bool b = std::get<bool>(val.data);
}
```

### QuantumPointer

The `QuantumPointer` struct represents a pointer to another `QuantumValue`. It includes a shared pointer (`cell`) to the actual value, a variable name (`varName`), and an offset (`offset`). This structure is particularly useful for managing references and facilitating pointer arithmetic.

#### Example Usage:
```cpp
QuantumPointer ptr = {std::make_shared<QuantumValue>(42), "myVar"};
int value = ptr.deref().as<int>();
```

### QuantumNativeFunc and QuantumNative

These structures define native functions and their names. `QuantumNativeFunc` is a function object that takes a vector of `QuantumValue`s and returns a `QuantumValue`. `QuantumNative` combines a function name with its corresponding function object, allowing for easy registration and invocation of native functions.

#### Example Usage:
```cpp
QuantumNativeFunc add = [](const std::vector<QuantumValue>& args) -> QuantumValue {
    return args[0].as<int>() + args[1].as<int>();
};
QuantumNative nativeFunc = {"add", add};
```

## Tradeoffs

While the design choices in `Value.h` offer flexibility, safety, and performance benefits, they also introduce some tradeoffs:

- **Complexity**: Using `std::variant` and custom structs increases the complexity of the codebase, requiring careful handling of type information and conversions.
- **Memory Overhead**: Shared pointers and variant types do introduce some memory overhead compared to simpler data structures.
- **Performance Impact**: Although modern compilers optimize these constructs well, there may still be slight performance impacts compared to more straightforward approaches.

Despite these tradeoffs, the design of `Value.h` has been chosen to provide a solid foundation for the QuantumLanguage compiler, balancing functionality, safety, and performance requirements effectively.