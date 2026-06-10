# `compileBinary` Function

## Overview

The `compileBinary` function is responsible for compiling binary expressions encountered during the compilation process of the Quantum Language. It handles various types of binary operators, including logical operations (`and`, `&&`, `or`, `||`, `??`), membership operations (`in`, `not in`), arithmetic operations (`+`, `-`, `*`, `/`, `%`, `//`, `**`), comparison operations (`==`, `!=`, `<`, `<=`, `>`, `>=`), and bitwise operations (`&`, `|`, `^`, `<<`, `>>`). The function ensures that the left and right operands of the expression are compiled correctly before applying the operator.

## Parameters

- `e`: A reference to a `BinaryExpression` object representing the binary expression to be compiled.

## Return Value

This function does not explicitly return a value. Instead, it compiles the binary expression by emitting appropriate bytecode instructions.

## Edge Cases

1. **Logical Operations**: When encountering logical operations (`and`, `&&`, `or`, `||`, `??`), the function compiles the left operand first. If the operation is `and` or `&&`, it emits a jump instruction to skip the right operand if the left operand evaluates to false. Similarly, for `or` or `||`, it jumps over the right operand if the left operand evaluates to true. For the null-coalescing operator (`??`), it behaves similarly to `or` but also checks if the left operand is null.
   
2. **Membership Operations**: For membership operations (`in`, `not in`), the function loads the `__contains__` method from the global scope, compiles both the left and right operands, and then calls the `__contains__` method. If the operation is `not in`, it negates the result of the call.

3. **Arithmetic and Comparison Operations**: These operations are straightforward. The function compiles both operands and then emits the corresponding bytecode instruction based on the operator.

4. **Bitwise Operations**: Similar to arithmetic and comparison operations, the function compiles both operands and emits the appropriate bytecode instruction for bitwise operations.

5. **Unknown Operators**: If the function encounters an unknown binary operator, it throws a runtime error indicating the unrecognized operator.

## Interactions with Other Components

- **Bytecode Emission**: The `emit` function is used to generate bytecode instructions. This includes loading global variables, popping values from the stack, calling functions, and performing arithmetic and logical operations.
  
- **Jump Instructions**: The `emitJump` function generates jump instructions. For logical operations, these jumps allow the function to skip unnecessary computations based on the evaluation of the left operand. The `patchJump` function updates the jump target after the subsequent code has been emitted.

- **Error Handling**: The function uses exception handling to manage errors, specifically when encountering an unknown binary operator. This ensures that the compiler can gracefully handle unexpected input and provide meaningful error messages.

## Implementation Details

The implementation of the `compileBinary` function involves several key steps:

1. **Handling Logical Operations**: For `and` and `&&`, the function compiles the left operand and emits a jump to skip the right operand if the left evaluates to false. For `or` and `||`, it emits a jump to skip the right operand if the left evaluates to true. The null-coalescing operator (`??`) behaves similarly to `or` but also checks for null.

2. **Handling Membership Operations**: The function loads the `__contains__` method from the global scope, compiles both operands, and then calls the method. Depending on whether the operation is `in` or `not in`, it either returns the result directly or negates it.

3. **Handling Arithmetic and Comparison Operations**: The function compiles both operands and looks up the corresponding bytecode instruction in the `opMap` unordered map. It then emits this instruction.

4. **Error Handling**: If the operator is not found in the `opMap`, the function throws a runtime error indicating the unrecognized operator.

Here is the complete implementation of the `compileBinary` function:

```cpp
void compileBinary(BinaryExpression& e) {
    int line = e.line;

    if (e.op == "and" || e.op == "&&") {
        compileExpr(*e.left);
        size_t sc = emitJump(Op::JUMP_IF_FALSE, line);
        emit(Op::POP, 0, line);
        compileExpr(*e.right);
        patchJump(sc);
        return;
    }
    if (e.op == "or" || e.op == "||" || e.op == "??") {
        compileExpr(*e.left);
        size_t sc = emitJump(Op::JUMP_IF_TRUE, line);
        emit(Op::POP, 0, line);
        compileExpr(*e.right);
        patchJump(sc);
        return;
    }
    if (e.op == "in" || e.op == "not in") {
        emit(Op::LOAD_GLOBAL, addStr("__contains__"), line);
        compileExpr(*e.left);
        compileExpr(*e.right);
        emit(Op::CALL, 2, line);
        if (e.op == "not in")
            emit(Op::NOT, 0, line);
        return;
    }

    compileExpr(*e.left);
    compileExpr(*e.right);

    static const std::unordered_map<std::string, Op> opMap = {
        {"+", Op::ADD},
        {"-", Op::SUB},
        {"*", Op::MUL},
        {"/", Op::DIV},
        {"%", Op::MOD},
        {"//", Op::FLOOR_DIV},
        {"**", Op::POW},
        {"==", Op::EQ},
        {"!