# QuantumLanguage Compiler - Value.h

## Overview

The `include/Value.h` header file is a crucial component of the QuantumLanguage compiler, responsible for defining and managing various data types and values within the language. It forms the backbone for the interpreter's ability to handle expressions, statements, and function calls efficiently. By utilizing C++'s type system and features such as `std::variant`, this file ensures that all possible value types can be encapsulated within a single unified structure, facilitating robust and flexible value management throughout the compilation process.

## Key Design Decisions

### Use of `std::variant`

**Why:** The primary reason for choosing `std::variant` over traditional unions or custom type-checking mechanisms is to leverage C++'s strong type system and safety features. `std::variant` allows for compile-time type checking and eliminates runtime errors associated with union misuse, ensuring that the compiler can catch type-related issues early and prevent undefined behavior.

### Non-Owning Pointers for AST Nodes

**Why:** To maintain memory efficiency and avoid unnecessary copying, `QuantumFunction` uses non-owning pointers (`ASTNode*`) for the function body instead of owning them (`std::shared_ptr`). This decision minimizes memory overhead while still allowing the interpreter to access the function's body during execution.

### Shared Ownership for Complex Types

**Why:** For complex types such as arrays, dictionaries, functions, natives, instances, classes, and pointers, shared ownership (`std::shared_ptr`) is used. This approach ensures that these objects can be safely passed around between different parts of the compiler without causing dangling references or premature deallocation.

## Classes and Functions Documentation

### QuantumNil

**Purpose:** Represents the nil value in QuantumLanguage, similar to `null` in other languages.

**Behavior:** A simple struct with no members, indicating the absence of a value.

### QuantumFunction

**Purpose:** Encapsulates information about a user-defined function in QuantumLanguage.

**Behavior:**
- Contains the function's name.
- Stores parameters along with their pass-by-value/pass-by-reference status.
- Holds default arguments for each parameter.
- References the function's body via a non-owning pointer (`ASTNode*`).
- Maintains a closure (`std::shared_ptr<Environment>`) containing the environment in which the function was defined.

### QuantumClass

**Purpose:** Represents a user-defined class in QuantumLanguage.

**Behavior:** A forward declaration, as the actual implementation details are not provided in this header file.

### QuantumInstance

**Purpose:** Encapsulates an instance of a user-defined class in QuantumLanguage.

**Behavior:** A forward declaration, as the actual implementation details are not provided in this header file.

### QuantumNative

**Purpose:** Represents a native function written in C++ that can be invoked from QuantumLanguage code.

**Behavior:**
- Contains the native function's name.
- Stores a callable object (`QuantumNativeFunc`) representing the native function itself.

### QuantumNativeFunc

**Purpose:** A type alias for a callable object that represents a native function.

**Behavior:** A function pointer taking a vector of `QuantumValue` objects and returning a `QuantumValue`.

### QuantumPointer

**Purpose:** Represents a pointer to a variable in QuantumLanguage, capable of handling both direct and indirect references.

**Behavior:**
- Contains a live reference to the variable storage (`std::shared_ptr<QuantumValue>`).
- Stores the variable's name for display and debugging purposes.
- Supports pointer arithmetic through an `offset` member.

**Trade-offs/Limitations:**
- Dereferencing a null pointer throws an exception, which may lead to performance overhead compared to unchecked access.
- The use of shared ownership can increase memory usage due to reference counting.

### QuantumValue

**Purpose:** The central structure for encapsulating any value in QuantumLanguage, providing a unified interface for accessing and manipulating values of different types.

**Behavior:**
- Utilizes `std::variant` to store values of various types including `QuantumNil`, booleans, doubles, strings, arrays, dictionaries, functions, natives, instances, classes, and pointers.
- Provides constructors for initializing values of different types.
- Offers methods for accessing and modifying the stored value, including dereferencing pointers.

## Tradeoffs and Limitations

- **Memory Overhead:** Using `std::shared_ptr` for complex types increases memory usage due to reference counting.
- **Performance Impact:** Exception handling for null pointer dereferences can introduce some performance overhead.
- **Complexity:** The inclusion of multiple value types and their corresponding constructors adds complexity to the codebase.

This header file plays a pivotal role in the QuantumLanguage compiler by providing a comprehensive framework for managing values and data types. Its design choices ensure type safety, memory efficiency, and flexibility, enabling the interpreter to execute QuantumLanguage code accurately and efficiently.