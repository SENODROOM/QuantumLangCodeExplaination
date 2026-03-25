# QuantumLanguage Compiler Expressions Module

## Overview

The `src/compiler/CompilerExpressions.cpp` file is a crucial component of the QuantumLanguage compiler, responsible for compiling expressions into intermediate representation (IR) instructions that can be executed on the virtual machine (`Vm`). This module handles both binary and unary operations, as well as assignment statements, ensuring efficient and correct compilation of these fundamental constructs.

## Role in Compiler Pipeline

This module operates at a critical stage in the compiler's pipeline, following the parsing phase where expressions are abstractly represented. It translates these representations into executable IR code, which is then further processed by subsequent stages such as optimization and code generation.

## Key Design Decisions and Why

### Expression Types Handling

The module distinguishes between binary and unary expressions and treats them differently. Binary expressions are further categorized based on their operators, while unary expressions handle simple negation, logical not, bitwise not, and increment/decrement operations.

### Operator Mapping

A `std::unordered_map` named `opMap` is used to map string representations of binary operators to corresponding IR operations. This allows for easy lookup and ensures that the compiler can handle a wide range of arithmetic and comparison operations without manual case handling.

### Compound Assignment Support

Compound assignments like `+=`, `-=` are supported by mapping them to their respective binary operations and adding an additional operation to update the variable. This approach simplifies the implementation and maintains consistency with standard language semantics.

### Error Handling

The module includes robust error handling using `throw std::runtime_error`. If an unknown operator is encountered during compilation, an exception is thrown with a descriptive message, preventing the compiler from proceeding with incorrect or incomplete code.

## Major Classes/Functions Overview

### `Compiler::compileBinary(BinaryExpr &e, int line)`

Compiles a binary expression into IR code. Handles logical AND/OR operations by short-circuiting, arithmetic operations, and membership tests.

### `Compiler::compileUnary(UnaryExpr &e, int line)`

Compiles a unary expression into IR code. Supports negation, logical not, bitwise not, and increment/decrement operations.

### `Compiler::compileAssign(AssignExpr &e, int line)`

Compiles an assignment expression into IR code. Handles regular assignments and compound assignments by converting them into sequence of IR operations.

## Tradeoffs

### Short-Circuit Evaluation

For logical AND (`&&`) and OR (`||`) operations, short-circuit evaluation is implemented. This means that the right-hand side of the expression is only evaluated if necessary, potentially improving performance but complicating the implementation.

### Simplified Compound Assignments

Compound assignments are handled by converting them into binary operations followed by an update operation. While this simplifies the code, it may introduce additional overhead compared to direct support for compound assignments.

### Error Handling Overhead

The use of exceptions for error handling adds some overhead compared to other methods. However, it provides a clear and consistent way to report errors, making debugging easier.

Overall, the `CompilerExpressions.cpp` file efficiently handles the compilation of various types of expressions, balancing simplicity, performance, and robustness.