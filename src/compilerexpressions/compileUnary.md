# `compileUnary` Function

## Purpose
The `compileUnary` function is responsible for compiling unary expressions in the Quantum Language compiler. Unary expressions include operations such as negation (`-`), logical not (`!`, `not`), bitwise not (`~`), and increment/decrement (`++`, `--`). This function ensures that these operations are correctly translated into intermediate representation (IR) instructions that can be executed by the quantum virtual machine.

## Parameters
- `e`: A reference to an `Expression` object representing the unary expression to be compiled.

## Return Value
This function does not explicitly return any value. Instead, it modifies the IR code by emitting appropriate instructions based on the unary operator encountered.

## Edge Cases
1. **Ellipsis Operator (`...`)**:
   - If the unary operator is `...`, the function compiles the operand expression and returns immediately without emitting any additional instructions. The ellipsis operator typically indicates a spread operation or similar construct, which does not require further processing in the context of unary operators.

2. **Increment/Decrement Operators (`++`, `--`)**:
   - These operators modify the value of the operand in-place. The function emits a constant load instruction for the value `1.0`, followed by either an addition or subtraction instruction depending on whether the operator is `++` or `--`. After modifying the value, it duplicates the modified value, stores it back into the identifier (if the operand is an identifier), and then pops the duplicate from the stack.

3. **Unknown Unary Operator**:
   - If the unary operator is not recognized (`-`, `!`, `not`, `~`, `++`, `--`), the function throws a `std::runtime_error` indicating an unknown unary operator.

## Interactions with Other Components
- **Intermediate Representation (IR) Emission**: The `emit` function is used to generate IR instructions. This interaction is crucial as it translates the high-level unary operations into low-level instructions that the quantum virtual machine can execute.
  
- **Constant Pool Management**: The `addConst` function manages the constant pool, ensuring that constants like `1.0` are stored efficiently and reused when needed. This helps in optimizing memory usage during compilation.

- **Expression Compilation**: The function calls `compileExpr` recursively to compile the operand expression before applying the unary operator. This ensures that the operand is properly evaluated and its result is available for the unary operation.

- **Code Generation Context**: The `line` parameter provides the current line number in the source code, which is used for error reporting and debugging purposes. This interaction allows the compiler to maintain accurate line information in the generated IR.

## Detailed Explanation
Here's a step-by-step breakdown of how the `compileUnary` function processes different unary operators:

1. **Ellipsis Operator (`...`)**:
   ```cpp
   if (e.op == "...")
   {
       compileExpr(*e.operand);
       return;
   }
   ```
   - If the operator is `...`, the function simply compiles the operand expression using `compileExpr(*e.operand)` and returns. No additional instructions are emitted because the ellipsis operator does not involve any unary operations.

2. **Negation (`-`)**:
   ```cpp
   else if (e.op == "-")
       emit(Op::NEG, 0, line);
   ```
   - For the negation operator, the function emits an `Op::NEG` instruction. This instruction negates the top value on the stack.

3. **Logical Not (`!`, `not`)**:
   ```cpp
   else if (e.op == "!" ||
            e.op == "not")
       emit(Op::NOT, 0, line);
   ```
   - Both `!` and `not` operators are treated equivalently, and the function emits an `Op::NOT` instruction to perform logical negation on the top value on the stack.

4. **Bitwise Not (`~`)**:
   ```cpp
   else if (e.op == "~")
       emit(Op::BIT_NOT, 0, line);
   ```
   - The bitwise not operator `~` is handled by emitting an `Op::BIT_NOT` instruction, which performs a bitwise NOT operation on the top value on the stack.

5. **Increment/Decrement Operators (`++`, `--`)**:
   ```cpp
   else if (e.op == "++" || e.op == "--")
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
   - For increment (`++`) and decrement (`--`) operators, the function first loads the constant `1.0` onto the stack using `Op::LOAD_CONST`.
   - It then adds or subtracts `1.0` from the top value on the stack using `Op::ADD` or `Op::SUB`, respectively.
   - If the operand is an identifier, the function duplicates the modified value using `Op::DUP`, stores it back into the identifier using `emitStore`, and finally pops the duplicate from the stack using `Op::POP`.

6. **Unknown Unary Operator**:
   ```cpp
   else
       throw std::runtime_error("Compiler: unknown unary op '" + e.op + "'");
   ```
   - If the unary operator is not one of the recognized types, the function throws an exception indicating an unknown unary operator. This helps