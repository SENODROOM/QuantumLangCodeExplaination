# `compileUnary` Function

## Purpose
The `compileUnary` function is responsible for compiling unary expressions within the Quantum Language compiler. It processes various unary operators including negation (`-`), logical not (`!`, `not`), bitwise not (`~`), and increment/decrement (`++`, `--`). The function ensures that the appropriate quantum operation is emitted based on the operator encountered.

## Parameters/Return Value
- **Parameters**:
  - `e`: A reference to an `Expression` object representing the unary expression to be compiled.
  
- **Return Value**: None. The function directly modifies the output stream by emitting quantum operations.

## How It Works
The function first checks if the unary operator is the spread operator (`...`). If so, it compiles the operand expression without any additional processing. For other unary operators, it proceeds as follows:

1. **Compile Operand Expression**: The function calls `compileExpr(*e.operand)` to recursively compile the operand of the unary expression.
2. **Emit Appropriate Operation**:
   - If the operator is `-`, it emits the `Op::NEG` operation to perform negation.
   - If the operator is `!` or `not`, it emits the `Op::NOT` operation to perform logical negation.
   - If the operator is `~`, it emits the `Op::BIT_NOT` operation to perform bitwise negation.
   - If the operator is `++` or `--`, it performs the following steps:
     - Emits the `Op::LOAD_CONST` operation to load the constant value `1.0` onto the stack.
     - Depending on whether the operator is `++` or `--`, it either emits the `Op::ADD` or `Op::SUB` operation to increment or decrement the top value on the stack.
     - If the operand is an identifier (i.e., a variable), it duplicates the top value on the stack using `Op::DUP`, stores it back into the variable using `emitStore()`, and then pops the duplicated value off the stack using `Op::POP`.

## Edge Cases
- **Spread Operator (`...`)**: When encountering the spread operator, the function simply compiles the operand without any modification, as it represents a different type of operation (e.g., unpacking elements).
- **Unknown Unary Operator**: If the function encounters an unknown unary operator, it throws a `std::runtime_error` indicating that the compiler does not support the specified operator.

## Interactions With Other Components
- **Expression Compilation**: The `compileUnary` function relies on the `compileExpr` function to handle the compilation of the operand expression. This recursive approach allows the compiler to break down complex expressions into simpler sub-expressions.
- **Stack Operations**: The function uses stack-based operations (`Op::LOAD_CONST`, `Op::ADD`, `Op::SUB`, `Op::DUP`, `Op::POP`) to manipulate values during compilation. These operations ensure that the correct order of evaluation and manipulation is maintained.
- **Variable Storage**: When dealing with increment/decrement operations on identifiers, the function interacts with the storage system to update the variable's value. This involves duplicating the current value, storing the new incremented/decremented value, and then popping the old value from the stack.

Overall, the `compileUnary` function plays a crucial role in handling unary operations within the Quantum Language compiler, ensuring that the correct quantum operations are emitted and that the program state is updated appropriately.