# QuantumLanguage Compiler Expressions Module

## Overview

The `src/compiler/CompilerExpressions.cpp` file is a critical component of the QuantumLanguage compiler, responsible for translating expressions into Intermediate Representation (IR) instructions that can be executed on the Virtual Machine (`Vm`). This module handles both binary and unary operations, as well as assignment statements, ensuring that the resulting IR accurately represents the semantics of the original code.

### Role in Compiler Pipeline

This module operates within the broader context of the QuantumLanguage compiler's pipeline. It follows the parsing stage, where expressions are syntactically analyzed, and proceeds to the optimization phase. The primary responsibilities include:

- **Binary Operations**: Compiling expressions involving operators like `+`, `-`, `*`, `/`, etc., into corresponding IR instructions.
- **Unary Operations**: Translating expressions with operators such as `-`, `!`, `~`, etc., into their respective IR forms.
- **Assignment Statements**: Converting assignment expressions into IR instructions that handle variable storage and retrieval efficiently.

By handling these fundamental expression types, the module ensures that the subsequent stages of the compiler can generate optimized machine code that closely mirrors the intended behavior of the source code.

## Key Design Decisions and Why

### Use of IR Instructions

The decision to use IR instructions instead of directly generating machine code allows for easier optimization and portability across different hardware architectures. IR provides a high-level abstraction that abstracts away many details of specific machine languages, making it possible to apply general optimizations before finalizing the target code.

### Operator Mapping

A key design decision is the use of an operator mapping table (`opMap`) to translate string representations of operators into their corresponding IR opcodes. This approach simplifies the implementation by centralizing the logic for operator translation, reducing redundancy and potential errors.

```cpp
static const std::unordered_map<std::string, Op> opMap = {
    {"+", Op::ADD},
    {"-", Op::SUB},
    // ... other mappings
};
```

### Compound Assignment Handling

Compound assignments (like `+=`, `-=`, etc.) are handled by normalizing them to their standard form (`=`) and then applying the appropriate operation. This normalization makes the code cleaner and more maintainable, as all assignment-related logic is concentrated in one place.

```cpp
const std::string normalizedOp =
    e.op == "post+=" ? "+=" : e.op == "post-=" ? "-="
                                               : e.op;
bool compound = (normalizedOp != "=");
```

## Major Classes/Functions Overview

### `Compiler::compileBinary(BinaryExpr &e, int line)`

Compiles binary expressions into IR instructions. Handles logical operators (`and`, `or`, `??`), comparison operators, arithmetic operators, and bitwise operators. Uses an operator mapping table to find the correct opcode for each operator.

### `Compiler::compileUnary(UnaryExpr &e, int line)`

Translates unary expressions into IR instructions. Supports negation (`-`), logical not (`!`, `not`), bitwise not (`~`), and increment/decrement operators (`++`, `--`). Handles special cases like the spread operator (`...`).

### `Compiler::compileAssign(AssignExpr &e, int line)`

Converts assignment expressions into IR instructions. Manages simple assignments (`=`) and compound assignments (`+=`, `-=`, etc.). Normalizes compound assignments to their standard form for consistency.

## Tradeoffs

### Abstraction vs. Performance

Using IR instructions introduces a layer of abstraction between the compiler's frontend and backend. While this improves flexibility and optimization opportunities, it may slightly increase the overhead compared to direct machine code generation. However, the benefits in terms of code quality and maintainability often outweigh this cost.

### Complexity vs. Simplicity

The introduction of an operator mapping table adds complexity to the codebase, but it significantly simplifies the process of compiling various operators. By centralizing this logic, the module becomes more readable and easier to extend or modify.

### Memory Usage vs. Execution Speed

Storing the operator mapping table in memory requires additional space. However, the performance impact is minimal, and the benefits in terms of code organization and maintainability make this tradeoff worthwhile.

In conclusion, the `src/compiler/CompilerExpressions.cpp` file plays a crucial role in the QuantumLanguage compiler by managing the compilation of expressions into IR instructions. Its design decisions, including the use of IR instructions and operator mapping tables, strike a balance between flexibility, performance, and simplicity, contributing to the overall effectiveness of the compiler.