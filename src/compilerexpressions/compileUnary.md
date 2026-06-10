# `compileUnary` Function

## Purpose
The `compileUnary` function is responsible for compiling unary expressions within the Quantum Language compiler. It processes various unary operators including negation (`-`), logical not (`!`, `not`), bitwise not (`~`), and increment/decrement (`++`, `--`). This function ensures that each unary operator is correctly translated into the corresponding quantum instruction set, facilitating efficient execution of quantum programs.

## Parameters
- `e`: A reference to an `Expression` object representing the unary expression to be compiled. The `Expression` object contains information about the operator (`op`) and its operand (`operand`).

## Return Value
This function does not explicitly return a value; instead, it modifies the internal state of the compiler by emitting quantum instructions. These instructions are added to the current program being compiled.

## Edge Cases
1. **Unsupported Unary Operator**: If the unary operator encountered is not supported by the Quantum Language compiler (i.e., not `-`, `!`, `not`, `~`, `++`, or `--`), the function throws a `std::runtime_error`. This ensures that any unexpected operators are flagged early in the compilation process, preventing silent failures later on.
2. **Increment/Decrement on Non-Identifier Operand**: The function checks if the operand of the `++` or `--` operator is an identifier. If not, it throws a `std::runtime_error`. This prevents errors related to attempting to modify non-identifier values, which would be invalid in the context of quantum programming.

## Interactions with Other Components
- **Emission of Instructions**: The `emit` function is called multiple times within `compileUnary` to generate quantum instructions based on the unary operator. For example:
  - `Op::NEG` for negation.
  - `Op::NOT` for logical not.
  - `Op::BIT_NOT` for bitwise not.
  - `Op::ADD` or `Op::SUB` for increment and decrement respectively.
- **Compilation of Operands**: Before processing the unary operator, the function calls `compileExpr` on the operand. This ensures that the operand itself is fully compiled before applying the unary operation. The `compileExpr` function handles different types of expressions, including binary expressions and literals, ensuring that all parts of the unary expression are correctly processed.
- **Handling Identifiers**: When dealing with the `++` or `--` operators, the function duplicates the operand using `Op::DUP`, stores the modified value back into the identifier using `emitStore`, and then pops the temporary value using `Op::POP`. This sequence ensures that the original value is preserved while the incremented/decremented value is used in the expression.

## Implementation Details
Here's a breakdown of how the `compileUnary` function handles different unary operators:

1. **Negation (`-`)**:
   ```cpp
   emit(Op::NEG, 0, line);
   ```
   This emits a `NEG` instruction, which negates the value at the top of the stack.

2. **Logical Not (`!`, `not`)**:
   ```cpp
   emit(Op::NOT, 0, line);
   ```
   This emits a `NOT` instruction, which performs a logical NOT operation on the value at the top of the stack.

3. **Bitwise Not (`~`)**:
   ```cpp
   emit(Op::BIT_NOT, 0, line);
   ```
   This emits a `BIT_NOT` instruction, which performs a bitwise NOT operation on the value at the top of the stack.

4. **Increment (`++`)**:
   ```cpp
   emit(Op::LOAD_CONST, addConst(QuantumValue(1.0)), line);
   emit(Op::ADD, 0, line);
   emitStore(e.operand->as<Identifier>().name, line);
   emit(Op::POP, 0, line);
   ```
   This sequence first loads the constant `1.0` onto the stack, adds it to the value at the top of the stack, stores the result back into the identifier, and finally pops the temporary value.

5. **Decrement (`--`)**:
   ```cpp
   emit(Op::LOAD_CONST, addConst(QuantumValue(1.0)), line);
   emit(Op::SUB, 0, line);
   emitStore(e.operand->as<Identifier>().name, line);
   emit(Op::POP, 0, line);
   ```
   Similar to increment, but uses the `SUB` instruction instead of `ADD`.

6. **Unknown Unary Operator**:
   ```cpp
   throw std::runtime_error("Compiler: unknown unary op '" + e.op + "'");
   ```
   This exception handling ensures that any unsupported unary operators are caught and reported as errors.

By carefully handling each unary operator and interacting with other components like `compileExpr` and `emit`, the `compileUnary` function provides robust support for unary expressions in the Quantum Language compiler, ensuring correct translation and execution of quantum programs.