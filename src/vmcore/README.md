# vmcore

The `vmcore` component is a crucial module within the Quantum Language compiler, responsible for executing bytecode and managing runtime operations efficiently. This directory includes several well-documented functions and files that work together to maintain the virtual machine's state and ensure smooth execution.

## Overview

The `vmcore` component handles the fundamental aspects of the virtual machine, including running bytecode, pushing and popping values from the stack, managing runtime errors, converting values to numbers, comparing values, and executing binary and unary operations. It also manages upvalues, closures, native function calls, class methods, and built-in method invocations.

### Key Functions

- **run**: The primary function that drives the execution of the virtual machine.
- **push**: Adds a value to the stack.
- **pop**: Removes a value from the stack.
- **runtimeError**: Handles runtime errors and halts the execution.
- **toNumber**: Converts a value to a number.
- **valuesEqual**: Compares two values for equality.
- **execBinary**: Executes binary operations like addition or multiplication.
- **execUnary**: Executes unary operations like negation or increment.
- **captureUpvalue**: Captures upvalues in closure creation.
- **closeUpvalues**: Closes upvalues when a function returns.
- **callValue**: Calls a function or method using a value.
- **callClosure**: Calls a function using a closure.
- **callNativeFn**: Invokes a native function.
- **callClass**: Calls a constructor for a class.
- **callBuiltinMethod**: Invokes a built-in method on an object.

### Directory Structure

```
vmcore/
├── bytecode.cpp
│   ├── run.cpp
│   └── ...
├── stack.cpp
│   ├── push.cpp
│   ├── pop.cpp
│   └── ...
├── error.cpp
│   ├── runtimeError.cpp
│   └── ...
├── conversion.cpp
│   ├── toNumber.cpp
│   └── ...
├── comparison.cpp
│   ├── valuesEqual.cpp
│   └── ...
├── operations.cpp
│   ├── execBinary.cpp
│   ├── execUnary.cpp
│   └── ...
├── upvalues.cpp
│   ├── captureUpvalue.cpp
│   ├── closeUpvalues.cpp
│   └── ...
├── call.cpp
│   ├── callValue.cpp
│   ├── callClosure.cpp
│   ├── callNativeFn.cpp
│   ├── callClass.cpp
│   └── callBuiltinMethod.cpp
└── ...
```

## How Files Relate to Each Other

1. **bytecode.cpp**: Contains the implementation of the `run` function, which orchestrates the execution of bytecode.
2. **stack.cpp**: Implements the stack operations (`push`, `pop`) used during the execution of bytecode.
3. **error.cpp**: Manages runtime errors through the `runtimeError` function.
4. **conversion.cpp**: Provides functionality to convert values to numbers using the `toNumber` function.
5. **comparison.cpp**: Includes the `valuesEqual` function for comparing values.
6. **operations.cpp**: Houses the implementations of binary (`execBinary`) and unary (`execUnary`) operations.
7. **upvalues.cpp**: Manages upvalues with `captureUpvalue` and `closeUpvalues`.
8. **call.cpp**: Contains various functions related to calling values, closures, native functions, classes, and built-in methods.

## Overall Flow

1. **Initialization**: The virtual machine initializes its state, including setting up the stack and loading bytecode.
2. **Bytecode Execution**: The `run` function reads and executes bytecode instructions one by one.
3. **Stack Operations**: During execution, values are pushed onto and popped from the stack as required by the bytecode instructions.
4. **Error Handling**: If a runtime error occurs, it is caught and handled by the `runtimeError` function.
5. **Value Conversion**: Values need to be converted to numbers for certain operations, which is done using the `toNumber` function.
6. **Comparison**: Values are compared for equality using the `valuesEqual` function.
7. **Operations**: Binary and unary operations are executed based on the bytecode instructions, utilizing the `execBinary` and `execUnary` functions.
8. **Upvalue Management**: Upvalues are captured and closed appropriately during function calls and returns, managed by `captureUpvalue` and `closeUpvalues`.
9. **Function Calls**: Various types of function calls (value, closure, native, class, built-in) are handled by their respective functions in the `call.cpp` file.

By understanding and maintaining these components, developers can effectively extend and debug the Quantum Language compiler's virtual machine.