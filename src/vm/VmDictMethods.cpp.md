# VmDictMethods.cpp

## Role in Compiler Pipeline

The `VmDictMethods.cpp` file is part of the Quantum Language compiler's virtual machine (VM) component. It contains implementations for various methods that operate on dictionary objects (`Dict`). These methods include getting keys, values, items, checking membership, retrieving and setting values, deleting entries, clearing dictionaries, and obtaining their size. This file plays a crucial role in enabling dynamic operations on dictionary data structures within the quantum programming language environment.

## Key Design Decisions and WHY

### Method Dispatching
- **Function Overloading**: The implementation uses multiple conditional checks to dispatch different methods based on the function name (`m`). This approach ensures that each method can be handled efficiently without requiring additional parameters or complex routing logic.
- **Type Safety**: By using `std::shared_ptr<Dict>` and `std::vector<QuantumValue>`, the functions maintain type safety and allow for flexible argument passing.

### Error Handling
- **Exception Throwing**: When an unrecognized method is called, the function throws a `TypeError`. This provides clear feedback to the user about what went wrong, aiding in debugging and improving error resilience.
- **Default Behavior**: For methods like `get` with insufficient arguments, the function returns an empty `QuantumValue`. This prevents runtime errors and allows the program to handle missing values gracefully.

### Flexibility and Extensibility
- **String-Based Method Names**: Allowing methods to be specified as strings (`const std::string &m`) makes the system highly extensible. New methods can be added simply by extending the list of conditions in the function.
- **Optional Arguments**: Methods like `get` support optional default values, enhancing usability and reducing the need for explicit error handling in client code.

## Major Classes/Functions Overview

### Class: Dict
- **Purpose**: Represents a dictionary data structure in the VM.
- **Operations**: Provides functionalities such as insertion, deletion, lookup, and iteration over its elements.

### Function: callDictMethod
- **Parameters**:
  - `std::shared_ptr<Dict> dict`: A shared pointer to the dictionary object on which the method will be called.
  - `const std::string &m`: The name of the method to be executed.
  - `std::vector<QuantumValue> args`: A vector containing the arguments passed to the method.
- **Return Value**: Returns a `QuantumValue` representing the result of the method call.
- **Behavior**:
  - **Keys**: Retrieves all keys from the dictionary and returns them as an array.
  - **Values**: Retrieves all values from the dictionary and returns them as an array.
  - **Items/Entries**: Retrieves all key-value pairs from the dictionary and returns them as an array of arrays.
  - **Has/Contains/HasOwnProperty**: Checks if the dictionary contains a specific key.
  - **Get**: Retrieves the value associated with a specific key; supports optional default values.
  - **Set**: Sets or updates the value associated with a specific key.
  - **Delete**: Removes a specific key from the dictionary.
  - **Clear**: Empties the dictionary.
  - **Size/Length**: Returns the number of elements in the dictionary as a double.

## Tradeoffs

### Performance vs. Simplicity
- **Performance**: Using string-based method names and multiple conditional checks can impact performance, especially under high load.
- **Simplicity**: The simplicity of the current implementation makes it easy to extend and understand.

### Memory Usage
- **Memory Efficiency**: The use of `std::shared_ptr` helps manage memory more efficiently by allowing multiple parts of the program to share ownership of the same dictionary object.
- **Potential Overhead**: However, frequent use of smart pointers might introduce some overhead compared to raw pointers.

### Type Safety vs. Flexibility
- **Type Safety**: The strong typing enforced by `QuantumValue` ensures safe operations but may limit flexibility in certain scenarios.
- **Flexibility**: Supporting optional arguments and string-based method names increases flexibility at the cost of potential type mismatches.

Overall, the `VmDictMethods.cpp` file strikes a balance between functionality, performance, and memory management, making it suitable for the needs of the Quantum Language compiler's VM component.