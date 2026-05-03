# `compileUnary` Function

## Purpose
The `compileUnary` function compiles unary expressions in the Quantum Language compiler. It handles operations like negation (`-`), logical not (`!`, `not`), bitwise not (`~`), and increment/decrement (`++`, `--`). This function ensures that the correct quantum operations are emitted based on the unary operator encountered during compilation.

## Parameters
- `e`: A reference to a `UnaryExpression` object representing the unary expression to be compiled. The `UnaryExpression` object contains the operator (`op`) and its operand (`operand`).

## Return Value
This function does not explicitly return a value. Instead, it modifies the internal state of the compiler by emitting appropriate quantum operations.

## How It Works
1. **Check for Ellipsis Operator**: If the unary operator is `"..."`, the function calls `compileExpr` recursively on the operand. This is likely used for ellipsis expansion or similar constructs where further processing is required.

2. **Compile Operand**: Regardless of the operator, the function first compiles the operand using `compileExpr`. This step prepares the necessary quantum values or states that will be operated upon by the unary operator.

3. **Emit Quantum Operations**:
   - For negation (`-`), it emits the `Op::NEG` operation, which applies the negation to the topmost item on the stack.
   - For logical not (`!` or `not`), it emits the `Op::NOT` operation, which performs a logical NOT on the topmost item.
   - For bitwise not (`~`), it emits the `Op::BIT_NOT` operation, which applies a bitwise NOT to the topmost item.
   - For increment (`++`) or decrement (`--`), it follows these steps:
     - Emits a constant value of `1.0` using `Op::LOAD_CONST`.
     - Depending on whether the operator is `++` or `--`, it either adds or subtracts this constant from the topmost item on the stack using `Op::ADD` or `Op::SUB`.
     - If the operand is an identifier, it duplicates the result using `Op::DUP`, stores it back into the same identifier using `emitStore`, and then pops the duplicate from the stack using `Op::POP`.

4. **Error Handling**: If the unary operator is unrecognized, the function throws a `std::runtime_error` indicating an unknown unary operator.

## Edge Cases
- **Ellipsis Operator**: Handles special cases involving the ellipsis operator, ensuring that further processing is done correctly.
- **Increment/Decrement**: Correctly handles identifiers when performing increment or decrement operations, ensuring that the original value is updated in place.

## Interactions with Other Components
- **Compilation Context**: Uses the current compilation context to manage the stack and emit quantum operations.
- **Expression Compiler**: Calls `compileExpr` on the operand to prepare the quantum values before applying the unary operator.
- **Constant Manager**: Utilizes `addConst` to manage constant values, ensuring they are available for use in quantum operations.
- **Store Emitter**: Uses `emitStore` to update identifiers with new values after increment or decrement operations.

Overall, the `compileUnary` function plays a crucial role in translating unary expressions from the Quantum Language into corresponding quantum operations that can be executed by the compiler's backend. Its implementation ensures that all supported unary operators are handled correctly and efficiently, maintaining the integrity and correctness of the compiled quantum program.