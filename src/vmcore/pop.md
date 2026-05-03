# `pop` Function

## Overview

The `pop` function is a method within the `VmCore` class of the Quantum Language compiler's virtual machine core. This function is responsible for removing and returning the top element from the internal quantum computation stack (`stack_`). It plays a crucial role in managing the state of quantum computations by allowing the retrieval of the most recent result or value pushed onto the stack.

## Parameters

- None

## Return Value

- Returns a `QuantumValue`, which represents the topmost item that was removed from the stack.

## Edge Cases

1. **Empty Stack**: If the stack is empty when `pop` is called, the function throws a `RuntimeError` with the message "VM stack underflow". This ensures that there is always at least one element on the stack before attempting to remove it, preventing potential runtime errors.

2. **Large Stack Size**: The function operates efficiently even for large stacks due to its use of `std::vector`, which provides fast access and removal operations at the end of the container.

3. **Stack with Mixed Types**: Since `QuantumValue` can represent various types of quantum data (e.g., qubits, measurements), the `pop` function can handle a stack containing elements of different types.

## Interactions with Other Components

- **Execution Flow**: The `pop` function is typically used after an operation that has computed a result and needs to pass that result to another part of the program. For example, after performing a measurement, the result might be popped off the stack and used elsewhere.

- **Memory Management**: By moving the top element out of the stack using `std::move`, the function helps in transferring ownership without copying, potentially saving memory resources when dealing with large objects.

- **Error Handling**: The function includes error handling to manage the case where the stack is unexpectedly empty, ensuring robustness in the face of unexpected conditions during execution.

Here is the implementation of the `pop` function:

```cpp
QuantumValue VmCore::pop() {
    if (stack_.empty()) {
        throw RuntimeError("VM stack underflow");
    }
    QuantumValue v = std::move(stack_.back()); // Move the top element
    stack_.pop_back(); // Remove the top element from the stack
    return v; // Return the moved element
}
```

This implementation ensures that the stack is properly managed and that any attempt to pop from an empty stack results in an appropriate error being thrown.