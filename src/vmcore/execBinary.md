# `execBinary`

The `execBinary` function is an integral component of the Quantum Language compiler's virtual machine core (`VmCore.cpp`). Its primary responsibility is to execute binary operations between two quantum values (`L` and `R`) based on the specified operation type (`op`). This function ensures that operations are performed correctly, taking into account whether the operands are native types or instances of user-defined classes.

## Parameters

- `L_in`: The left operand as a `QuantumValue`.
- `R_in`: The right operand as a `QuantumValue`.
- `op`: The operation type as an enumeration value of type `Op`.

## Return Value

- Returns the result of the binary operation as a `QuantumValue`.

## How It Works

### Native Types Handling

When both operands are native types, the function directly performs the binary operation using the corresponding C++ operators. For example:

- If `op` is `Op::ADD`, the function uses the `+` operator to add the two native values.
- If `op` is `Op::MUL`, the function uses the `*` operator to multiply the two native values.

If either operand is not a native type but rather an instance of a user-defined class, the function attempts to find a corresponding "magic method" in the class definition. These magic methods are named according to the operation type (e.g., `__add__` for addition).

### Magic Methods Dispatching

If the left operand (`L`) is an instance of a user-defined class, the function iterates through the class hierarchy starting from the most derived class up to its base classes. For each class, it checks if the class has defined a method corresponding to the operation type (`op`). If such a method is found, the function creates a `QuantumBoundMethod` object, which binds the method to the current instance (`L`). It then pushes this bound method onto the stack along with the operands (`L` and `R`). The function calls the closure associated with the method, passing the operands as arguments, and runs the resulting frame.

### String Concatenation

For string concatenation (when `op` is `Op::ADD`), the function specifically handles the case where at least one of the operands is a string. If both operands are strings, it concatenates them using the `+` operator. If only one operand is a string, it converts the other operand to a string representation before concatenating.

## Edge Cases

- **Mixed Operand Types**: When operands are of mixed types (one native, one instance), the function will attempt to use the appropriate magic method if available. If not, it may throw an error or perform implicit conversions.
- **Class Instances Without Magic Methods**: If a class instance does not define a magic method for the specified operation, the function will skip to the next class in the hierarchy. If no suitable method is found, it may fall back to a default behavior or throw an error.
- **Empty Class Hierarchies**: If a class instance belongs to a class without any parent classes, the function will check only the methods defined in that class itself.

## Interactions with Other Components

- **Virtual Machine Core**: The `execBinary` function operates within the context of the virtual machine core, managing the execution stack and frames.
- **Quantum Bound Method**: When a magic method is dispatched, a `QuantumBoundMethod` object is created and pushed onto the stack. This object encapsulates the method and the instance it is bound to.
- **Error Handling**: The function includes mechanisms for handling errors, such as checking for the existence of magic methods and performing type conversions when necessary.

Overall, the `execBinary` function provides a robust mechanism for executing binary operations in the Quantum Language compiler, ensuring correct behavior for both native and user-defined types.