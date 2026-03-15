# QuantumLanguage Compiler - Value.h

## Overview

The `include/Value.h` header file is a crucial component of the QuantumLanguage compiler, responsible for defining the data structures and types that represent various values within the language. These values include basic types such as integers, doubles, strings, arrays, dictionaries, functions, native functions, instances, classes, and pointers. By managing these value semantics effectively, the compiler can accurately evaluate expressions, handle function calls, and maintain state during execution.

## Key Design Decisions

### Use of `std::variant`
**Why:** The use of `std::variant` allows for a single type (`QuantumValue`) to hold any one of several different types without requiring dynamic memory allocation. This choice enhances performance and reduces overhead compared to traditional union-based approaches.

### Non-Owning Pointers for Function Bodies
**Why:** By storing function bodies as non-owning pointers (`ASTNode* body`), the compiler avoids unnecessary copying of large AST nodes. This decision minimizes memory usage and improves efficiency, especially when dealing with complex programs.

### Shared Ownership for Environments and Pointers
**Why:** Using `std::shared_ptr` ensures that environments and pointers have shared ownership, preventing premature deallocation and enabling safe concurrent access. This approach is essential for maintaining correct behavior in multi-threaded scenarios.

## Classes and Functions Documentation

### QuantumNil
**Purpose:** Represents the nil value in QuantumLanguage.
**Behavior:** A simple struct with no members.

### QuantumFunction
**Purpose:** Represents a user-defined function in QuantumLanguage.
**Members:**
- `name`: The name of the function.
- `params`: A vector of parameter names.
- `paramIsRef`: A vector indicating whether each parameter should be passed by reference.
- `defaultArgs`: A vector of default argument expressions.
- `body`: A non-owning pointer to the function's body node.
- `closure`: A shared pointer to the environment in which the function was defined.

**Behavior:** Constructs a new function object with the specified parameters and body.

### QuantumClass
**Purpose:** Represents a user-defined class in QuantumLanguage.
**Members:** 
- Not explicitly shown in the snippet but would typically include information about class attributes, methods, and inheritance.

**Behavior:** Defines the structure of a class, including its properties and behaviors.

### QuantumInstance
**Purpose:** Represents an instance of a class in QuantumLanguage.
**Members:** 
- Not explicitly shown in the snippet but would typically include references to the class definition and instance-specific data.

**Behavior:** Manages the state of an object, allowing it to interact with the class's methods and attributes.

### QuantumNativeFunc
**Purpose:** A type alias for a native function in QuantumLanguage.
**Definition:** A `std::function` that takes a vector of `QuantumValue`s and returns a `QuantumValue`.

**Behavior:** Allows the integration of external C++ functions into the QuantumLanguage runtime.

### QuantumNative
**Purpose:** Represents a native function in QuantumLanguage.
**Members:**
- `name`: The name of the native function.
- `fn`: A `QuantumNativeFunc` representing the actual implementation.

**Behavior:** Wraps a native function, providing a way to call it from within the QuantumLanguage interpreter.

### QuantumPointer
**Purpose:** Represents a pointer to a variable in QuantumLanguage.
**Members:**
- `cell`: A shared pointer to the variable's storage.
- `varName`: The name of the variable being pointed to.
- `offset`: An integer offset for pointer arithmetic.

**Functions:**
- `isNull()`: Checks if the pointer is null.
- `deref()`: Dereferences the pointer, returning a reference to the underlying `QuantumValue`. Throws an exception if the pointer is null.

**Behavior:** Enables pointer operations within the QuantumLanguage runtime, supporting features like dynamic memory management and pointer arithmetic.

### QuantumValue
**Purpose:** Represents a generic value in QuantumLanguage.
**Members:**
- `data`: A `std::variant` containing one of several possible value types (`QuantumNil`, `bool`, `double`, etc.).

**Constructors:**
- Various constructors for initializing `QuantumValue` objects of different types.

**Behavior:** Provides a unified interface for handling values of multiple types, leveraging `std::variant` for efficient type-safe storage and retrieval.

## Tradeoffs and Limitations

- **Performance Overhead:** While `std::variant` offers improved performance, it may introduce some overhead compared to simpler unions due to its more complex type safety checks.
- **Complexity:** Managing multiple value types and their interactions through `std::variant` adds complexity to the codebase, potentially increasing maintenance costs.
- **Memory Usage:** Although `std::shared_ptr` helps reduce memory usage by sharing ownership, it still incurs additional overhead for reference counting and garbage collection.

These tradeoffs reflect the balance between type safety, performance, and simplicity required in a modern compiler.