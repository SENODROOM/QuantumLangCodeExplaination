# vmcore

The `vmcore` component is an essential part of the Quantum Language compiler, designed to execute bytecode and manage runtime operations efficiently. This directory houses several well-documented functions and files that collaborate seamlessly to maintain the virtual machine's state and ensure smooth execution.

## Overview

The `vmcore` component consists of core functionalities required for the execution of quantum programs. It handles the interpretation of bytecode, manages memory and data structures, and provides mechanisms for error handling and debugging.

## Key Components

### run
- **Description**: The primary function that initiates the execution of the virtual machine. It reads and processes bytecode instructions until completion or an error occurs.
- **Location**: `src/vmcore/run.cpp`

### push
- **Description**: Adds a value to the stack. Used during the evaluation of expressions.
- **Location**: `src/vmcore/stack.cpp`

### pop
- **Description**: Removes the top value from the stack. Used when evaluating expressions and popping intermediate results.
- **Location**: `src/vmcore/stack.cpp`

### runtimeError
- **Description**: Handles runtime errors, such as division by zero or invalid type conversions. It logs the error and halts the program execution.
- **Location**: `src/vmcore/error.cpp`

### toNumber
- **Description**: Converts a value to a number. Used in arithmetic operations where values need to be numeric.
- **Location**: `src/vmcore/conversion.cpp`

### valuesEqual
- **Description**: Compares two values for equality. Used in conditional statements and comparisons.
- **Location**: `src/vmcore/comparison.cpp`

### execBinary
- **Description**: Executes binary operations on two operands. Supports addition, subtraction, multiplication, etc.
- **Location**: `src/vmcore/execution.cpp`

### execUnary
- **Description**: Executes unary operations on a single operand. Supports negation, increment, decrement, etc.
- **Location**: `src/vmcore/execution.cpp`

### captureUpvalue
- **Description**: Captures upvalues (local variables from enclosing scopes) into closures. Ensures that local variables remain accessible even after the outer scope has exited.
- **Location**: `src/vmcore/closure.cpp`

### closeUpvalues
- **Description**: Closes upvalues, deallocating memory and updating references. Called when a closure goes out of scope.
- **Location**: `src/vmcore/closure.cpp`

### callValue
- **Description**: Calls a function or method using its value. Handles dynamic dispatch based on the object's type.
- **Location**: `src/vmcore/function.cpp`

### callClosure
- **Description**: Calls a closure, which is a function bundled with its lexical environment. Manages the call frame and upvalues.
- **Location**: `src/vmcore/closure.cpp`

### callNativeFn
- **Description**: Invokes native functions written in C++. These functions provide access to system resources or perform complex calculations not easily achievable in the quantum language itself.
- **Location**: `src/vmcore/native.cpp`

### callClass
- **Description**: Instantiates a new class and calls its constructor. Manages the creation of objects and initialization of their properties.
- **Location**: `src/vmcore/class.cpp`

### callBuiltinMethod
- **Description**: Calls built-in methods associated with objects. These methods are predefined and optimized for performance within the quantum language.
- **Location**: `src/vmcore/method.cpp`

## Directory Structure

```
src/
└── vmcore/
    ├── run.cpp
    ├── stack.cpp
    ├── error.cpp
    ├── conversion.cpp
    ├── comparison.cpp
    ├── execution.cpp
    ├── closure.cpp
    ├── function.cpp
    └── class.cpp
        └── method.cpp
            └── native.cpp
```

## Flowchart

```plaintext
+---------------------+
|      run()          |
|  - Starts VM        |
|  - Reads bytecode   |
|  - Processes ops    |
|  - Calls functions  |
|  - Handles errors   |
+---------+-----------+
          |
          v
+---------+-----------+
|     push()      |
|  - Adds value to stack|
+---------+-----------+
          |
          v
+---------+-----------+
|      pop()      |
|  - Removes value from stack|
+---------+-----------+
          |
          v
+---------+-----------+
|  runtimeError() |
|  - Logs error       |
|  - Halts execution  |
+---------+-----------+
          |
          v
+---------+-----------+
|    toNumber()     |
|  - Converts value to number|
+---------+-----------+
          |
          v
+---------+-----------+
| valuesEqual()   |
|  - Compares values  |
+---------+-----------+
          |
          v
+---------+-----------+
|  execBinary()   |
|  - Executes binary operations|
+---------+-----------+
          |
          v
+---------+-----------+
|  execUnary()    |
|  - Executes unary operations|
+---------+-----------+
          |
          v
+---------+-----------+
| captureUpvalue()|
|  - Captures upvalues|
+---------+-----------+
          |
          v
+---------+-----------+
| closeUpvalues() |
|  - Closes upvalues  |
+---------+-----------+
          |
          v
+---------+-----------+
|   callValue()   |
|  - Calls function/value|
+---------+-----------+
          |
          v
+---------+-----------+
|  callClosure()  |
|  - Calls closure    |
+---------+-----------+
          |
          v
+---------+-----------+
| callNativeFn()