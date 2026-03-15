# QuantumLanguage Compiler - Value.h

## Overview

The `include/Value.h` header file serves as a fundamental building block for the QuantumLanguage compiler, defining data structures and types that represent values within the language. This includes basic types like integers, doubles, strings, arrays, dictionaries, functions, native functions, instances, classes, and pointers. By managing these value semantics effectively, the compiler can accurately evaluate expressions, handle function calls, and maintain state across different stages of compilation.

## Key Design Decisions

### Use of `std::variant`
The decision to use `std::variant` for the `QuantumValue::Data` type was driven by the need for a flexible and type-safe way to store multiple possible value types in a single container. This approach avoids the overhead of dynamic casting while providing compile-time safety and ensuring that all potential value types are accounted for.

### Smart Pointers
Utilizing smart pointers (`std::shared_ptr`) for managing memory ensures automatic deallocation when a value is no longer needed, reducing the risk of memory leaks. Additionally, shared ownership allows for efficient sharing of resources between different parts of the compiler without manual reference counting.

### Non-Owning Pointers for Function Bodies
Choosing non-owning pointers (`ASTNode*`) for storing function bodies in the `QuantumFunction` struct optimizes memory usage by avoiding unnecessary duplication of nodes. This decision balances the need for quick access to function definitions during interpretation against the cost of managing additional pointers.

## Documentation

### QuantumNil
**Purpose:** Represents the `nil` value in QuantumLanguage.
**Behaviour:** A simple empty structure indicating the absence of a value.

### QuantumFunction
**Purpose:** Encapsulates information about a user-defined function.
**Members:**
- `name`: The name of the function.
- `params`: A vector of parameter names.
- `paramIsRef`: A vector indicating whether each parameter should be passed by reference.
- `defaultArgs`: A vector of default argument values.
- `body`: A non-owning pointer to the function's body node.
- `closure`: A shared pointer to the environment in which the function was defined.

**Behaviour:** Stores details necessary for interpreting a function call.

### QuantumClass
**Purpose:** Represents a user-defined class in QuantumLanguage.
**Members:**
- Not fully defined in the provided snippet.

**Behaviour:** Placeholder for class-related data structures.

### QuantumInstance
**Purpose:** Represents an instance of a user-defined class.
**Members:**
- Not fully defined in the provided snippet.

**Behaviour:** Placeholder for instance-related data structures.

### QuantumNativeFunc
**Purpose:** A type alias for a native function in QuantumLanguage.
**Type:** `std::function<QuantumValue(std::vector<QuantumValue>)>`
**Behaviour:** Defines a callable entity that takes a vector of `QuantumValue`s and returns a `QuantumValue`.

### QuantumNative
**Purpose:** Encapsulates information about a native function.
**Members:**
- `name`: The name of the native function.
- `fn`: A pointer to the function implementation.

**Behaviour:** Allows the compiler to invoke native functions seamlessly.

### QuantumPointer
**Purpose:** Represents a pointer to a variable in QuantumLanguage.
**Members:**
- `cell`: A shared pointer to the actual variable storage.
- `varName`: The name of the variable for display/debug purposes.
- `offset`: An integer used for pointer arithmetic.

**Functions:**
- `isNull() const`: Checks if the pointer is null.
- `deref() const`: Dereferences the pointer, throwing an exception if it is null.

**Behaviour:** Manages references to variables, enabling pointer arithmetic and debugging capabilities.

### QuantumValue
**Purpose:** A versatile container for holding any valid value in QuantumLanguage.
**Members:**
- `data`: A variant containing one of several possible value types.

**Constructors:**
- Various constructors are provided for initializing `QuantumValue` with different types of data.

**Behaviour:** Acts as a polymorphic wrapper around various value types, facilitating easy manipulation and storage of values throughout the compiler pipeline.

## Tradeoffs and Limitations

- **Memory Overhead:** Using smart pointers introduces some memory overhead compared to raw pointers.
- **Performance:** Dynamic dispatch through `std::variant` can introduce slight performance penalties compared to static typing.
- **Complexity:** Managing multiple value types and their associated operations adds complexity to the compiler's codebase.

These tradeoffs reflect the balance between flexibility, safety, and performance required in a modern compiler.