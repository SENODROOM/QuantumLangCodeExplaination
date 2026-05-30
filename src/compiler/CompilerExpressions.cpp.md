# QuantumLanguage Compiler Expressions Module

## Overview

The `src/compiler/CompilerExpressions.cpp` file is a crucial component of the QuantumLanguage compiler, responsible for translating expressions into Intermediate Representation (IR) instructions that can be executed on the Virtual Machine (`Vm`). This module handles both binary and unary operations, as well as assignment statements, ensuring efficient compilation and execution of quantum programs.

### Role in Compiler Pipeline

This module operates within the compiler's expression handling phase. It takes parsed expressions and converts them into IR instructions that are then passed to the virtual machine for execution. The primary responsibilities include:
- Compiling binary expressions like arithmetic, comparison, logical, and bitwise operations.
- Compiling unary expressions such as negation, logical NOT, bitwise NOT, increment/decrement, and identity operators.
- Handling assignment expressions, including simple assignments and compound assignments.

### Key Design Decisions and Why

#### Expression Mapping

A key design decision is the use of an unordered map to translate string representations of operators into corresponding IR operations. This approach allows for quick lookup and ensures that new operations can be easily added without modifying existing code. For example:

```cpp
static const std::unordered_map<std::string, Op> opMap = {
    {"+", Op::ADD},
    {"-", Op::SUB},
    // ... other mappings
};
```

#### Compound Assignments

Handling compound assignments (like `a += b`) requires special processing to ensure that the operation is performed correctly and efficiently. The module uses a mapping to normalize these operations and then compiles them accordingly. For instance:

```cpp
static const std::unordered_map<std::string, Op> cops = {
    {"+=", Op::ADD},
    {"-=", Op::SUB},
    // ... other mappings
};
```

#### Error Handling

Proper error handling is essential to maintain robustness. The module throws exceptions when encountering unknown operators, providing clear feedback and enabling easier debugging. This ensures that the compiler can gracefully handle errors and provide meaningful messages to the user.

### Major Classes/Functions Overview

#### `Compiler::compileBinary(BinaryExpr &e, int line)`

This function compiles binary expressions. It supports various operations such as arithmetic, logical, and bitwise operations. If the operation involves short-circuiting (like `and` or `or`), it emits conditional jumps and pops unnecessary values from the stack. For example:

```cpp
if (e.op == "and" || e.op == "&&")
{
    compileExpr(*e.left);
    size_t sc = emitJump(Op::JUMP_IF_FALSE, line);
    emit(Op::POP, 0, line);
    compileExpr(*e.right);
    patchJump(sc);
    return;
}
```

#### `Compiler::compileUnary(UnaryExpr &e, int line)`

This function compiles unary expressions. It supports basic unary operations like negation, logical NOT, bitwise NOT, and identity operators. Additionally, it handles increment and decrement operations, which may involve loading constants and performing arithmetic. For example:

```cpp
if (e.op == "++" || e.op == "--")
{
    emit(Op::LOAD_CONST, addConst(QuantumValue(1.0)), line);
    emit(e.op == "++" ? Op::ADD : Op::SUB, 0, line);
    if (e.operand->is<Identifier>())
    {
        emit(Op::DUP, 0, line);
        emitStore(e.operand->as<Identifier>().name, line);
        emit(Op::POP, 0, line);
    }
}
```

#### `Compiler::compileAssign(AssignExpr &e, int line)`

This function compiles assignment expressions. It supports simple assignments and compound assignments, normalizing the latter to their equivalent forms. The function ensures that the correct IR operations are emitted for each type of assignment. For example:

```cpp
const std::string normalizedOp =
    e.op == "post+=" ? "+=" : e.op == "post-=" ? "-="
                                               : e.op;
bool compound = (normalizedOp != "=");

static const std::unordered_map<std::string, Op> cops = {
    {"+=", Op::ADD},
    {"-=", Op::SUB},
    // ... other mappings
};

auto it = cops.find(normalizedOp);
if (it != cops.end())
    emit(it->second, 0, line);
else
    throw std::runtime_error("Compiler: unknown assignment op '" + e.op + "'");
```

### Tradeoffs

#### Performance vs. Flexibility

Using an unordered map for operator translation provides fast lookups but limits flexibility in adding new operations. Balancing performance with ease of extension could involve using a more complex data structure or additional parsing logic.

#### Code Size vs. Readability

Compiling compound assignments into simpler forms reduces the generated IR code size but might sacrifice some readability. Ensuring that the code remains readable while maintaining efficiency is a challenge.

#### Error Handling vs. Robustness

While throwing exceptions for unknown operators provides immediate feedback, it might lead to less intuitive error messages. Implementing a more sophisticated error reporting system could improve usability.

In conclusion, the `src/compiler/CompilerExpressions.cpp` file plays a vital role in the QuantumLanguage compiler by translating expressions into executable IR instructions. Its design choices balance performance, flexibility, and robustness, making it a critical component of the overall compilation process.