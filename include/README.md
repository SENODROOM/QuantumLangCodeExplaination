# QuantumLanguage Compiler - Value.h

## Overview

The `include/Value.h` header file plays a pivotal role in the QuantumLanguage compiler by defining and managing various data types and values within the language. This file is essential for the interpreter to handle expressions, statements, and function calls efficiently. It leverages C++'s type system to create a flexible and robust representation of values, ensuring that the compiler can manage different data types seamlessly.

### Key Design Decisions and Why

1. **Use of `std::variant`**: The primary choice here is the use of `std::variant` to encapsulate multiple value types in a single object. This allows for compile-time polymorphism without the overhead of virtual functions, making the code more efficient and easier to understand.

2. **Separate Structures for Different Types**: Each data type is represented by a separate structure (`QuantumNil`, `QuantumFunction`, etc.). This separation makes it clear what each type represents and simplifies the implementation of operations on these types.

3. **Shared Pointers for Memory Management**: By using `std::shared_ptr`, the file ensures proper memory management for dynamically allocated objects. This helps prevent memory leaks and dangling pointers, which are common issues in manual memory management.

4. **Pass-by-Reference Support**: The `QuantumFunction` struct includes a vector indicating whether parameters should be passed by reference or not. This feature is crucial for supporting mutable references in the language, enhancing its expressiveness and performance.

5. **Flexibility with Native Functions**: The inclusion of `QuantumNativeFunc` and `QuantumNative` structures allows the compiler to integrate native C++ functions into the language. This flexibility is important for leveraging existing libraries and optimizing performance-critical parts of the program.

## Major Classes/Functions Overview

### QuantumValue

This class is the central representation of all values in the QuantumLanguage. It uses `std::variant` to store different types of data, including basic types like `bool` and `double`, complex types like arrays and dictionaries, and user-defined types like functions and instances.

#### Constructors

- `QuantumValue()`: Initializes the value as `nil`.
- `explicit QuantumValue(bool b)`: Initializes the value as a boolean.
- `explicit QuantumValue(double d)`: Initializes the value as a number.
- `explicit QuantumValue(const std::string &s)`: Initializes the value as a string.
- `explicit QuantumValue(std::string &&s)`: Initializes the value as a move-constructed string.
- `explicit QuantumValue(std::shared_ptr<Array> a)`: Initializes the value as an array.
- `explicit QuantumValue(std::shared_ptr<Dict> d)`: Initializes the value as a dictionary.
- `explicit QuantumValue(std::shared_ptr<QuantumFunction> f)`: Initializes the value as a function.
- `explicit QuantumValue(std::shared_ptr<QuantumNative> n)`: Initializes the value as a native function.
- `explicit QuantumValue(std::shared_ptr<QuantumInstance> i)`: Initializes the value as an instance.
- `explicit QuantumValue(std::shared_ptr<QuantumClass> c)`: Initializes the value as a class.
- `explicit QuantumValue(std::shared_ptr<QuantumPointer> p)`: Initializes the value as a pointer.

#### Type Checks

- `bool isNil() const`: Returns true if the value is `nil`.
- `bool isBool() const`: Returns true if the value is a boolean.
- `bool isNumber() const`: Returns true if the value is a number.

### QuantumPointer

This structure represents a pointer to a variable in the QuantumLanguage. It contains a shared pointer to the actual variable storage (`cell`) and additional metadata such as the variable's name (`varName`) and an offset for pointer arithmetic.

#### Methods

- `bool isNull() const`: Returns true if the pointer is null.
- `QuantumValue &deref() const`: Dereferences the pointer and returns the value it points to. Throws a runtime error if the pointer is null.

### QuantumFunction

This structure represents a function in the QuantumLanguage. It includes details such as the function's name, parameters, default arguments, body, and closure environment.

#### Members

- `std::string name`: The name of the function.
- `std::vector<std::string> params`: A list of parameter names.
- `std::vector<bool> paramIsRef`: Indicates whether each parameter should be passed by reference.
- `std::vector<ASTNode*> defaultArgs`: Default argument values for the function.
- `ASTNode *body`: The body of the function (non-owning pointer).
- `std::shared_ptr<Environment> closure`: The environment in which the function was defined.

## Trade-offs

1. **Memory Overhead**: Using `std::variant` and `std::shared_ptr` introduces some memory overhead compared to traditional unions and raw pointers. However, this trade-off is justified by the benefits of type safety and automatic memory management.

2. **Performance**: While `std::variant` offers compile-time polymorphism, it may introduce slight runtime overhead due to the need for type checking. However, this is generally outweighed by the benefits of a cleaner and safer API.

3. **Complexity**: The introduction of multiple structures and the use of `std::variant` adds complexity to the codebase. However, this complexity is manageable and leads to a more maintainable and scalable system.

Overall, the `include/Value.h` header file is designed to provide a comprehensive and efficient way to represent and manage values in the QuantumLanguage. Its use of modern C++ features like `std::variant` and `std::shared_ptr` ensures both type safety and optimal performance, making it a critical component of the compiler's architecture.