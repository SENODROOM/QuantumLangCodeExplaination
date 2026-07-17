# `execBinary`

The `execBinary` function is a crucial component of the Quantum Language compiler's virtual machine core (`VmCore.cpp`). Its primary responsibility is to execute binary operations between two quantum values (`L` and `R`) based on the specified operation type (`op`). This function ensures that operations are handled correctly, whether they involve built-in types or custom class instances.

## What it Does

- **Dispatch to Magic Methods**: If the left operand (`L`) is an instance of a class and the operation type corresponds to a defined method in that class (e.g., `__add__` for addition), the function will look up and invoke the appropriate magic method.
- **Built-in Operations**: For basic arithmetic and comparison operations involving strings or arrays, the function performs the operation directly using built-in C++ functionalities.

## Why it Works This Way

### Dispatch to Magic Methods

This approach allows for extensibility and polymorphism in handling binary operations. By defining magic methods within classes, users can customize how their objects interact with each other. The function checks if the left operand is an instance and then looks up the corresponding magic method in its class hierarchy. If found, it invokes the method with the right operand as an argument.

### Built-in Operations

For fundamental types like strings and arrays, direct use of C++ operators simplifies implementation and improves performance. This avoids the overhead of invoking external methods and leverages the efficiency of built-in operations.

## Parameters/Return Value

- **Parameters**:
  - `L`: A `QuantumValue` representing the left operand.
  - `R`: A `QuantumValue` representing the right operand.
  - `op`: An enumeration value of type `Op`, specifying the type of binary operation to be executed.
  - `line`: An integer indicating the line number where the operation occurs, used for error reporting.

- **Return Value**:
  - Returns a `QuantumValue` containing the result of the binary operation.

## Edge Cases

- **Non-Instance Left Operand**: If `L` is not an instance of a class, the function will only handle built-in operations.
- **Missing Magic Method**: If the specified magic method is not found in the class hierarchy, the function will skip execution and may need further handling depending on the context.
- **Type Mismatch**: When performing operations on incompatible types (e.g., adding a string to an array), the function should return an appropriate error or exception.

## Interactions with Other Components

- **Class System**: The function interacts with the class system to resolve and invoke magic methods.
- **Error Handling**: Errors related to missing methods or incompatible types are propagated through the function's return mechanism, which might be caught and handled by higher-level components.
- **Stack Management**: The function uses stack operations (`push`, `pop`) to manage intermediate results during the execution of binary operations.

This comprehensive approach ensures that the `execBinary` function can handle both simple and complex binary operations efficiently and flexibly, catering to the diverse needs of the Quantum Language compiler's virtual machine.