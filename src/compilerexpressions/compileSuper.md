# `compileSuper`

## Purpose

The `compileSuper` function in the Quantum Language compiler is responsible for handling the compilation of references to the superclass. This function primarily addresses two scenarios:
1. **Standalone `super()` Reference**: When the code includes a standalone reference to `super`.
2. **Standalone `super.method` Access**: When the code includes a standalone reference to a method of the superclass (`super.method`).

For `super.method()` calls, the `compileCall` function handles them directly.

## Parameters

- `e`: An expression object that contains information about the `super` reference being compiled. This object typically includes details such as whether the reference is to a method or not.
- `line`: The current line number in the source code where the `super` reference appears. This is used for error reporting and debugging purposes.

## Return Value

This function does not explicitly return a value; instead, it emits bytecode instructions using the `emit` function.

## How It Works

### 1. Emitting "self" Load Instruction

The function starts by emitting an instruction to load the `self` variable onto the stack. This represents the current instance of the class, which is necessary to access its superclass.

```cpp
emitLoad("self", line);
```

### 2. Handling Method References

If the expression object `e` indicates that the `super` reference is to a method (`!e.method.empty()`), the function proceeds to emit an instruction to get the superclass's method.

```cpp
if (!e.method.empty())
    emit(Op::GET_SUPER, addStr(e.method), line);
```

- `Op::GET_SUPER`: This is an opcode representing the operation to retrieve a method from the superclass.
- `addStr(e.method)`: This function adds the method name to the string table and returns its index. This index is then used as an argument to the `GET_SUPER` opcode.
- `line`: This parameter provides the line number in the source code, aiding in error reporting and debugging.

## Edge Cases

- **Empty Method Name**: If the method name in the expression object `e` is empty, the function assumes it is a standalone `super()` reference and only loads the `self` variable without attempting to access any method.
- **Invalid Superclass**: If the superclass does not exist or cannot be accessed during compilation, the function should ideally raise an error. However, the existing implementation does not include explicit error handling for these cases.

## Interactions with Other Components

- **Emit Function**: The `emitSuper` function interacts with the `emit` function to generate bytecode instructions. The `emit` function is likely defined in another part of the compiler, possibly in a header file like `src/compiler/CompilerUtils.h`.
  
- **String Table Management**: The function uses `addStr(e.method)` to manage the string table. This interaction is crucial for ensuring that all method names are correctly stored and referenced throughout the compilation process.

In summary, the `compileSuper` function plays a vital role in compiling references to the superclass within the Quantum Language compiler. By loading the `self` variable and optionally retrieving a method from the superclass, it facilitates the correct execution of superclass-related operations in the compiled code.