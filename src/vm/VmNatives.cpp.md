# VmNatives.cpp - Native Functions Registration Module for Quantum Language Compiler

## Overview
`VmNatives.cpp` is a crucial component of the Quantum Language compiler's virtual machine (VM) module. It handles the registration of native functions that can be invoked within the quantum programming environment. These native functions provide essential capabilities such as input/output operations, mathematical calculations, string manipulations, and more. By integrating these native functions into the VM, the Quantum Language compiler enables developers to write more powerful and efficient quantum programs.

## Role in Compiler Pipeline
The `VmNatives.cpp` file operates during the compilation phase of the Quantum Language compiler. Specifically, it is responsible for:
- **Registering Native Functions**: It defines and registers native functions with the VM, making them accessible to users through the quantum programming language.
- **Handling Input/Output Operations**: It includes implementations for native functions that facilitate user interaction, such as reading input from the console or displaying output on the screen.
- **Performing Mathematical Calculations**: It provides native functions for performing common mathematical operations, enhancing the computational power of quantum programs.
- **Supporting String Manipulations**: It offers native functions for manipulating strings, enabling developers to handle text data effectively within their quantum applications.

## Key Design Decisions and Why
### Use of Shared Pointers
In `VmNatives.cpp`, shared pointers (`std::shared_ptr`) are used to manage the lifecycle of native function objects. This decision ensures that native functions remain valid even after they have been registered, preventing potential memory leaks and dangling references.

### Error Handling
The file incorporates robust error handling mechanisms, such as throwing exceptions when expected types are not met. For example, the `toNum2` function checks if the provided value is a number and throws a `TypeError` if it is not. This approach helps maintain the integrity and reliability of the VM by ensuring that only valid inputs are processed.

### Format-Aware Input
The `defaultTestInputValue` function supports format-aware input, allowing native functions to handle different data formats based on the prompt provided. This feature enhances the flexibility and usability of native functions, making them suitable for various scenarios within quantum programming.

## Major Classes/Functions Overview
### `VM` Class
The `VM` class is the central class in `VmNatives.cpp`. It encapsulates the functionality required to register and execute native functions. The `registerNatives` method is a key member of this class, responsible for defining and registering all available native functions.

### `QuantumNativeFunc` Type
This type represents a native function in the Quantum Language compiler. It is a callable entity that takes a vector of `QuantumValue` arguments and returns a `QuantumValue`.

### `QuantumNative` Struct
The `QuantumNative` struct holds information about a native function, including its name and implementation. It is used internally by the `VM` class to manage and track native functions.

### `globals` Object
The `globals` object is a global variable that stores the definitions of all native functions. It allows for easy access and invocation of these functions within the VM.

## Tradeoffs
### Memory Management
Using shared pointers for managing native function objects introduces additional overhead compared to raw pointers. However, it significantly improves memory safety and reduces the risk of memory leaks, which is essential for a reliable virtual machine.

### Performance vs. Flexibility
While the format-aware input feature enhances the usability of native functions, it may introduce some performance overhead due to increased complexity in parsing and handling different data formats. Balancing performance with flexibility is a challenge in designing such systems.

### Error Handling Overhead
Implementing comprehensive error handling mechanisms adds overhead to the execution process. However, it ensures that the VM remains robust and capable of handling unexpected situations gracefully, improving overall program stability.

By carefully considering these tradeoffs, the Quantum Language compiler aims to strike a balance between functionality, safety, and performance, providing developers with a powerful and flexible platform for building quantum applications.