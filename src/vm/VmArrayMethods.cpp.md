# VmArrayMethods.cpp - Array Methods Implementation

## Overview

`VmArrayMethods.cpp` is an essential part of the Quantum Language compiler's virtual machine (VM) subsystem, dedicated to providing comprehensive array manipulation capabilities. This file encompasses core operations such as `push`, `pop`, `shift`, `unshift`, `reverse`, `sort`, `join`, `includes`, `contains`, and `indexOf`. These methods enable dynamic array handling within the VM, facilitating complex data manipulations required during compilation and execution phases.

## Role in Compiler Pipeline

The primary role of `VmArrayMethods.cpp` is to extend the functionality of arrays within the Quantum Language environment. By implementing these methods, it ensures that developers can perform various array operations directly from their code, enhancing productivity and efficiency. The methods are invoked through the VM's interpreter, allowing seamless integration into the overall compilation process.

## Key Design Decisions and Why

### Dynamic Method Dispatch

Each method is dispatched dynamically based on the operation requested. This approach allows for flexibility and scalability, accommodating different types of array elements and custom sorting criteria.

### Exception Handling

Exception handling is implemented for critical operations like `pop` and `indexOf`, ensuring robustness against errors such as popping from an empty array or accessing an invalid index. This prevents runtime crashes and provides clear error messages to the developer.

### Trade-offs

#### Memory Efficiency vs. Performance

Some operations, like `sort`, involve significant memory usage due to the creation of temporary storage for sorting algorithms. However, this trade-off is justified by the improved performance and reliability of sorted data structures.

#### Readability vs. Complexity

While some methods, like `join`, offer straightforward implementations, others, such as `sort`, require more complex logic to handle both numeric and string comparisons. Balancing readability with complexity ensures maintainable and efficient code.

## Major Classes/Functions Overview

### `class Array`

Represents the quantum array type, inheriting from the base `QuantumValue` class. It encapsulates a vector of `QuantumValue` objects, providing methods to manipulate the array dynamically.

### `QuantumValue VM::callArrayMethod(std::shared_ptr<Array> arr, const std::string &m, std::vector<QuantumValue> args)`

This function serves as the entry point for calling array methods. It takes a shared pointer to an array, the method name, and a vector of arguments. Based on the method name, it invokes the corresponding implementation, handling exceptions and returning the result as a `QuantumValue`.

### `void Array::push_back(const QuantumValue &value)`

Adds an element to the end of the array. If no value is provided, it appends an empty `QuantumValue`.

### `QuantumValue Array::pop(int idx)`

Removes and returns an element at the specified index. Throws a runtime error if the index is out of bounds.

### `double Array::size()`

Returns the number of elements in the array as a double.

### `QuantumValue Array::shift()`

Removes the first element from the array and returns it. Returns an empty `QuantumValue` if the array is empty.

### `void Array::unshift(const QuantumValue &value)`

Inserts an element at the beginning of the array.

### `void Array::reverse()`

Reverses the order of elements in the array.

### `void Array::sort()`

Sorts the elements in the array. Uses a custom comparator that handles both numeric and string values.

### `std::string Array::join(const std::string &separator)`

Concatenates all elements in the array into a single string, separated by the provided separator.

### `bool Array::includes(const QuantumValue &value)`

Checks if the array contains the specified value. Returns true if found, false otherwise.

### `double Array::indexOf(const QuantumValue &value)`

Finds the index of the specified value in the array. Returns -1 if not found.

## Conclusion

`VmArrayMethods.cpp` plays a pivotal role in enabling powerful array operations within the Quantum Language compiler's VM subsystem. Through its well-designed functions and exception handling, it enhances the language's usability and robustness, making it easier for developers to work with dynamic data structures efficiently.